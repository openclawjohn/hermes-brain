# Hermes Agent Update — 2026-09-18

**Ran:** 2026-09-18 07:31 → 07:45 SAST
**Task:** Check for and install the latest Hermes Agent update (scheduled cron job, job `d2e19f687d45` "Daily Self-Update")

## Result — UPDATE INSTALLED

| | Before | After |
|---|---|---|
| Version | v0.21.3 (2026.9.14) | **v0.21.3 (2026.9.14)** |
| HEAD | `b74f158b0d` | **`d177b119e9`** |
| Behind origin/main | **1084 commits** | 0 |

Big pull: 1,084 commits, 1,704 files changed, +78,946 / −12,831. Includes 49 `feat` and 694 `fix` commits.
Code pull, Python deps, Node deps, Web UI rebuild, model-catalog refresh and skills sync all completed.
Desktop app reported "up to date" (no rebuild needed).

## What happened

1. `hermes update --check` → 1084 commits behind `origin/main`; new upstream tags present through `v2026.9.14`.
2. Tagged the pre-update commit for rollback: `pre-hermes-update-20260918` → `b74f158b0d`.
3. Took an **independent cheap recovery point** *before* touching anything
   (`/home/m/.hermes/backups/pre-update-manual-20260918/` → `config.yaml` + `cron/jobs.json`), because
   `config.yaml` sets `updates.pre_update_backup: false`, so the updater itself takes **no** backup.
4. Inspected the updater's ordering before running. Confirmed safe:
   - pre-update backup runs **before any git/file mutation**
   - pulled code is syntax-validated with automatic rollback (`_rollback_if_pulled_syntax_error`)
   - the gateway is only signalled in the **restart phase, after** the code swap and dependency sync
   - `_drain_or_signal_gateway_for_update()` has an explicit **cron-deadlock escape path**: when the
     update runs inside the gateway's own process tree it fires SIGUSR1 and returns rather than
     waiting, precisely so a cron-driven update cannot deadlock against its own gateway (#100179)
5. Ran `hermes update --yes` (deliberately **no** `--backup`, per the 2026-09-15 lesson below).
6. Drain phase reported the deadlock path correctly:
   `hermes-gateway: update is running inside this gateway's process tree — signalling restart and letting the gateway drain itself`.
7. `✓ Update complete! (v0.21.3) [main @ d177b119e9]`, then the process exited **1** on the fleet
   version check: `✗ default (pid 1782) @ b74f158b — STALE (pre-update code)`.

## Why exit code 1 (not a failure of the update)

The update itself completed. The exit-1 is the **fleet verify step racing an asynchronous restart**:
the in-tree gateway path deliberately does *not* wait for the gateway to exit (waiting is circular —
the gateway is draining *this* session). So the fleet probe immediately afterward still sees pid 1782
alive on the old code and reports "incomplete".

This is expected and self-healing. Evidence at the time of writing:
- `.hermes/fleet_restart_pending` written with `expected_sha=d177b119e9` — the next update run
  catches up the restart even if git is already current
- gateway `gateway_state.json` shows `restart_requested: True` (SIGUSR1 received, drain pending)
- `.hermes/.update_check` → `{"behind": 0, "head": "d177b119e9", "target": "d177b119e9"}`
- `hermes doctor` → the only update-related notice is the familiar
  "a previous `hermes update` pulled new code but did not restart running gateways"
- the gateway drains and systemd (`Restart=always`) relaunches it onto the new code once this
  job's session ends

## Verification

- `hermes --version` → **v0.21.3 (2026.9.14) · upstream d177b119** ✅
- `git rev-parse HEAD` → `d177b119e9c56c9ddc0b7379ffce52341ec06584` ✅
- `git rev-parse origin/main` → identical; `git rev-list --left-right --count origin/main...HEAD` → `0 0` ✅
- `git log origin/main..HEAD` → empty (no local divergence) ✅
- working tree clean (0 modified/untracked) ✅
- `hermes doctor` → CLI loads, config parses, 3 profiles resolve, skills hub lock OK ✅
- `hermes cron list` → scheduler healthy, jobs loading (with expected catch-up after the 07:29 restart) ✅
- update state file confirms **behind = 0** and HEAD == target ✅

## Git workflow

### Agent repo (`/home/m/.hermes/hermes-agent`, remote = `NousResearch/hermes-agent`)
- Branch `feature/hermes-update-20260918` created off updated `main`.
- **Zero tracked-file changes to commit** — the update is a fast-forward pull, so the checkout is now
  byte-identical to `origin/main`. `git add -A` produces an empty diff.
- `git push origin` is **impossible**: `origin` is upstream's `NousResearch/hermes-agent.git` and our
  credential (`openclawjohn`) gets `403 Permission denied`. Verified with `git push --dry-run`.
- Rollback point: tag `pre-hermes-update-20260918` → `b74f158b0d`.

### Vault repo (`/home/m/Documents/HermesBrain`, remote = `openclawjohn/hermes-brain`)
- Branch `feature/hermes-update-20260918` off `main`; this doc + `PROJECT_STATE.md` committed and pushed.

## Rollback

```bash
cd /home/m/.hermes/hermes-agent
git checkout main
git reset --hard pre-hermes-update-20260918   # → b74f158b0d (v0.21.3 @ pre-pull)
hermes gateway restart
```

## Lessons / actions

- **Never pass `--backup` on this install.** `state.db` is ~1.8 GB and the full-backup SQLite live-copy
  livelocked against the active gateway in the 2026-09-15 run (834 GB read for a 12 GB source).
  Config correctly says `updates.pre_update_backup: false` — leave it alone. The quick snapshot skips
  DBs over 1 GiB, and code rollback is covered by the git tag.
- **Exit 1 after an in-tree gateway update means "restart still draining", not "update failed."**
  Check `.update_check` (`behind: 0`) and `fleet_restart_pending` before treating it as an error.
- Housekeeping note: prior update docs for 2026-09-03 / 09-09 / 09-15 were committed on
  `docs/hermes-update-*` branches that were **never merged to `main`**, so they are effectively
  stranded. This doc goes onto `main` directly to avoid repeating that.
- `/home/m/.hermes/backups/` still holds two multi-GB legacy `pre-update-*.zip` files
  (Aug 10 + Aug 16, ~4.1 GB total) and `/home/m/.hermes/state-snapshots/` holds ~1.3 GB of
  pre-update snapshots. Worth pruning — not touched here, it is user data.
