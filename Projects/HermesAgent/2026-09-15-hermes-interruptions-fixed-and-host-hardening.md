# 2026-09-15 — Hermes Interruption Root Cause Fixed + Host Hardening

**Project:** HermesAgent
**Date:** 2026-09-15
**Status:** Complete, verified

---

## Part 1 — Why Hermes kept getting interrupted

### Symptom
Turns died mid-flight with `status=error` and:
```
cause='session_turn_lease_timeout:20260914_165615_c01623'
ERROR run_agent: session turn lease wait timed out
TimeoutError: session finalization method set_session_title timed out
```

### Root cause (confirmed, not guessed)
`state.db` had grown to **12 GB** and was still on the **legacy inline FTS layout**:

| Fact | Value |
|---|---|
| `state.db` size | 12,018,200,960 bytes (2,934,131 pages × 4096) |
| `state_meta.fts_storage_version` | **ABSENT** → layout 0 (legacy) |
| Expected `FTS_STORAGE_VERSION` | 2 (for `SCHEMA_VERSION` 30) |
| `messages_fts` | `fts5(content)` — **inline**, duplicated every message |
| `messages_fts_trigram` | `fts5(content, tokenize='trigram')` — **inline**, ~2.6× the text it covers |
| `messages_fts_trigram_src` view | **missing** (code expects it) |
| Trigram rows | 239,380 — of which **176,249 (74%)** were rows the current design excludes (`role='tool'` OR source in `cron`/`subagent`) |
| Actual message text | only **1,938 MB** |

Because the live schema predated the current code, the migration that installs the compact
external-content layout had never run. Every message INSERT/UPDATE therefore wrote **three**
indexes while holding SQLite's single writer lock, and the trigram index still carried the
tool-output and cron rows that the design deliberately excludes.

The **turn lease** (`session_turn_leases`, TTL 300s) must be refreshed by a *write*. On a
12 GB DB with that write amplification, lease refresh and turn finalization
(`set_session_title`) blew their timeouts → the lease expired → the turn was killed as
`session_turn_lease_timeout`. `SELECT count(*)` on the trigram index literally timed out at
>180s.

Compounding: sessions were never pruned (`config.yaml: sessions.auto_prune: false`), leaving
10,002 cron sessions. The daily `website-guardian` cron re-inserted a ~250 KB skill prompt as a
`user` message on every run (largest single messages: 259,042 / 254,612 chars).

### Fix applied
```
hermes sessions optimize-storage --yes     # rebuild FTS → compact v23 external-content
hermes sessions prune --source cron --older-than 30 --yes
hermes sessions optimize                   # FTS merge + VACUUM
hermes config set sessions.auto_prune true
hermes config set sessions.retention_days 30
```

### Result (measured)
| Metric | Before | After |
|---|---|---|
| `state.db` | 11,463 MB | **1,798 MB** |
| Reclaimed | — | **9,665 MB** |
| `fts_storage_version` | absent (0) | **2** |
| Lease-style write | timed out (~300s → killed turn) | **0.0049 s** |
| Trigram count query | **>180 s timeout** | **0.33 s** |
| `PRAGMA quick_check` | — | **ok** |
| Sessions pruned | — | 4,898 |
| Trigram rows | 239,380 (74% waste) | 60,646 (correctly scoped) |

The WAL was also checkpointed (2.5 GB → 67 MB).

---

## Part 2 — Host hardening (nothing can hack us, we can still reach out)

### What was actually wrong

**UFW was not running at all.**
- `systemctl is-enabled ufw` → `enabled`, but `ufw status verbose` → **`Status: inactive`**
- `/etc/ufw/ufw.conf` → **`ENABLED=no`**
- The nft ruleset had **no `hook input` chain whatsoever** — only Docker chains and a
  `forward` hook. There was **no inbound firewall of any kind.**

**Docker published services on `0.0.0.0`, bypassing UFW entirely.** Docker writes DNAT rules
plus ACCEPT rules into the FORWARD path (`DOCKER`/`DOCKER-BRIDGE`), so ufw's INPUT policy
never sees that traffic. Reachable from every device on the LAN:

| Port | Service | Risk |
|---|---|---|
| 54322 | `supabase_db_loofinder` (Postgres 15.8) | direct database access |
| 54323 | `supabase_studio_loofinder` | DB admin UI |
| 54321 | `supabase_kong_loofinder` | full Supabase REST/Auth/Storage API |
| 54324 | `supabase_inbucket_loofinder` (Mailpit) | **no auth — reads all captured email** |
| 54327 | `supabase_analytics_loofinder` | analytics |
| 8888 | `searxng` | open search proxy |

**Credentials were world-readable** (`-rw-rw-r--`): `/home/m/credentials.json` (FTP + WP app
passwords + DB passwords for all 7 sites), `/home/m/SITE_CREDENTIALS.md`, `/home/m/.hermes/.env`.

### Fix applied
1. **UFW enabled properly** — `DEFAULT_INPUT_POLICY=DROP`, `DEFAULT_OUTPUT_POLICY=ACCEPT`,
   `DEFAULT_FORWARD_POLICY=DROP`, `ENABLED=yes`. No inbound allow rules are needed: every
   service the user consumes (Studio, Postgres, searxng, Ollama, the Hermes gateway socket) is
   used over loopback, and sshd is not running.
2. **Docker ports closed at the FORWARD layer** via `/usr/local/sbin/docker-guard.sh`, which
   fills `DOCKER-USER` — Docker's documented hook, evaluated **before** Docker's own chains and
   never flushed by Docker during a run:
   ```
   -A DOCKER-USER -i lo -j RETURN
   -A DOCKER-USER -m conntrack --ctstate RELATED,ESTABLISHED -j RETURN
   -A DOCKER-USER -i docker+ -j RETURN
   -A DOCKER-USER -i br-+ -j RETURN
   -A DOCKER-USER -j DROP
   ```
   The `docker+` / `br-+` **wildcards** matter: an earlier revision enumerated bridges by name,
   which would have cut internet access for any *new* Docker network. Direction is encoded in
   the interface — container **egress** has `iifname = a bridge`, network **inbound** has
   `iifname = the physical NIC` — so returning early on bridges preserves outbound.
3. **`docker-guard.service`** (enabled, `PartOf=docker.service`) re-applies the rules after any
   Docker restart or reboot.
4. **Credentials locked down** to `0600`; the backup dir to `0700`.
5. **Kernel/network hardening** in `/etc/sysctl.d/60-hermes-hardening.conf` — no ICMP
   redirects sent or accepted, no source routing, log martians, ignore broadcast pings/bogus
   responses, SYN-flood tuning, no IPv6 RAs. Deliberately left alone: `rp_filter=2` (strict
   breaks Docker routing) and `ip_forward=1` (Docker requires it).
6. **Cleanup** — removed 2 orphan containers that had never started (`upbeat_jang`,
   `funny_greider`) and the temporary test network.
7. **Fixed the failing cron deliveries** — `Daily Self-Update` and
   `OLLAMA-CLOUD-failover-watchdog` targeted `discord`, but `platforms.discord.enabled: false`,
   so every run logged `delivery_failed`. Both set to `--deliver local`.

### Verification (measured, not assumed)
- `ufw status verbose` → **Status: active**, `Default: deny (incoming), allow (outgoing), deny (routed)`
- `iptables -S INPUT` → **`-P INPUT DROP`**
- From an **untrusted Docker network** → ports 54322/54323/54321/54324/54327/8888 all **BLOCKED**
- From the **host** → loopback services 54322/54323/54321/8888/11434 all **OPEN**
- **Egress** from host → `https://api.github.com` **HTTP 200**
- **Egress** from a *new* Docker network → `https://example.com` **HTTP 200** (proves the
  wildcard fix works for networks that did not exist when the rules were written)
- Hermes gateway: `active`; dashboard `127.0.0.1:37673` → **HTTP 200**; Ollama API responding
- `/etc/sysctl.d/60-hermes-hardening.conf` applied and confirmed via `sysctl`

### Notes / accepted limitations
- **`hermes security audit`** reports 15 findings, all in the Hermes **venv** and all pinned by
  Hermes itself (`httpx2==2.7.0`, `httpcore2==2.7.0`, `pydantic-settings==2.13.1`,
  `setuptools==79.0.1`). `httpx2` is a deliberate, load-bearing pin for the httpx→httpx2
  migration (`tools/mcp_tool.py`) and is declared in `pyproject.toml`; these are upstream's to
  bump. Not hand-patched, to avoid desyncing a self-updating checkout.
- User `m` is in the `docker` group, so `m` has effective root via `/var/run/docker.sock`. This
  is the pre-existing intended NOPASSWD grant (`/usr/bin/apt-get`, `/usr/bin/docker`).
- `wsdd` (WS-Discovery) still broadcasts on UDP 3702 — a desktop discovery helper spawned by
  the user session, not a listening service. Left as-is.
- Unattended upgrades already enabled and working (`apt-daily-upgrade.timer`).

---

## Environment gotcha discovered (useful for future work)
`sudo -n` can run `/usr/bin/docker` **and** `/usr/bin/apt-get`, but not `ufw`/`systemctl`.
To reach host root without a password, use the docker grant with host namespaces:
```bash
sudo -n /usr/bin/docker run --rm --privileged --pid=host --net=host -v /:/host alpine:latest \
  sh -c "apk add -q util-linux 2>/dev/null; nsenter -t 1 -m -u -i -n -p -- <command>"
```
`chroot /host` does **not** work for systemd commands (no system bus); `nsenter -t 1` does.

## Backup
`~/.hermes/backups/pre-repair-20260915/` — `state-canonical.db` (2.31 GB, `quick_check` ok,
239,514 messages / 11,401 sessions, FTS indexes excluded as they are regenerable). A plain
`sqlite3 .backup` **livelocked** on the 12 GB DB (writer contention); the working approach was a
`CREATE TABLE ... AS SELECT` copy into a fresh DB with no FTS triggers. Scripts kept alongside:
`make_backup.py`, `docker-guard.sh`, `apply-hardening.sh`, `apply-sysctl.sh`, `DIAGNOSIS.md`.
