# PROJECT_STATE.md — Hermes Agent (self-hosted install)

**Last Updated:** 2026-09-18 07:45 SAST
**Purpose:** Source of truth for the Hermes Agent runtime install status on this machine.

---

## Current status

| Item | Value |
|---|---|
| Version | v0.21.3 (2026.9.14) |
| HEAD | `d177b119e9c56c9ddc0b7379ffce52341ec06584` |
| Branch | `main` (checkout == `origin/main`, 0 ahead / 0 behind) |
| Install dir | `/home/m/.hermes/hermes-agent` |
| Install method | git |
| Python | 3.11.15 (venv) |
| Upstream remote | `NousResearch/hermes-agent.git` (read-only for us — push = 403) |
| Profiles | default, developer, qc-auditor, ux-designer |
| Supervisor | `systemd --user` unit `hermes-gateway.service` (`Restart=always`) |

## Open items

- ⚠ **Gateway restart pending.** `fleet_restart_pending` marker written with
  `expected_sha=d177b119e9`; gateway `restart_requested: True`. Resolves automatically once the
  in-flight cron session ends and systemd relaunches. Manual shortcut: `hermes gateway restart`.
- ⚠ `state.db` is ~1.8 GB — `hermes doctor` recommends enabling `sessions.auto_prune`.
- ⚠ npm vulnerabilities: agent-browser (2), web workspace (6).
- ⚠ `ARCEEAI_API_KEY` check flagged in `.env`.
- 🧹 Disk: `/home/m/.hermes/backups/` holds ~4.1 GB of legacy `pre-update-*.zip`; 
  `/home/m/.hermes/state-snapshots/` holds ~1.3 GB. Prune candidates.

## Scheduled maintenance (cron)

| Job | ID | Schedule | Purpose |
|---|---|---|---|
| Daily Self-Update | `d2e19f687d45` | 03:00 daily | Runs `hermes update` |
| weekly-git-backup | — | Sun 03:00 | Auto-commit + push all 7 site repos |
| weekly-site-health-check | — | Mon 04:00 | Homepage/sitemap 200 checks |

## Key configuration to preserve

- `updates.pre_update_backup: false` — **intentional.** The 12 GB→1.8 GB `state.db` live-copy
  livelocks against an active gateway. Never force `--backup` on this install.
- `agent.restart_drain_timeout: 180`, `agent.gateway_timeout: 7200`.

## Rollback

```bash
cd /home/m/.hermes/hermes-agent
git checkout main
git reset --hard pre-hermes-update-20260918   # → b74f158b0d
hermes gateway restart
```

## Task history

| Date | Version | HEAD | Notes |
|---|---|---|---|
| 2026-09-18 | v0.21.3 | `d177b119e9` | 1084-commit pull; fleet-verify exit 1 = async drain, not failure |
| 2026-09-15 | v0.21.3 | `2179a279ae` | `--backup` caused full-backup livelock; re-ran `--no-backup` |
| 2026-09-09 | v0.21.3 | — | doc stranded on unmerged branch |
| 2026-09-03 | v0.21.0 | `593aa74c` | doc stranded on unmerged branch |
| 2026-08-31 | v0.20.6 | `1cf36398` | |
| 2026-07-02 | v0.18.0 | `76be77009` | |
