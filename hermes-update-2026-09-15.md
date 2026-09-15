# Hermes Agent Update — 2026-09-15

**Ran:** 2026-09-15 03:00 → 04:05 SAST
**Task:** Check for and install the latest Hermes Agent update (scheduled cron job, job `d2e19f687d45`)

## Result — UPDATE INSTALLED

| | Before | After |
|---|---|---|
| Version | v0.21.2 (2026.9.11) | **v0.21.3 (2026.9.14)** |
| HEAD | `5eb99eb284` | **`2179a279ae`** |
| Behind origin/main | 221 commits | 0 |

Code pull, dependency reinstall, Web UI rebuild and model-catalog/skills sync all completed.
Desktop app reported "up to date" (no rebuild needed).

## What happened (including a real failure)

1. `git fetch` → 221 commits behind `origin/main`, new tag `v2026.9.14`.
2. Tagged the pre-update commit: `pre-hermes-update-20260915` → `5eb99eb2`.
3. Ran `hermes update --yes --backup` detached under a transient `systemd --user` unit
   (so the gateway restart could not kill the updater mid-flight).
4. **The `--backup` flag was a mistake.** `config.yaml` already sets
   `updates.pre_update_backup: false` (off). Forcing a FULL pre-update backup made the updater
   take a WAL-safe live snapshot of `state.db` (12 GB) — and that copy livelocked:
   - destination frozen at exactly 5,025,824,768 bytes for 13+ minutes
   - **834 GB read** for a 12 GB source (SQLite `backup()` restart loop)
   - the gateway wrote to `state.db-wal` every ~30 s, so the copy never reached a consistent end
   - the "Pre-update snapshot: skipping state.db (11.1 GB exceeds 1.0 GB limit)" line shows the
     quick snapshot had *already* correctly decided to skip this DB — only the forced full zip tried it
5. Aborted the stuck unit, deleted the orphaned staging files (`tmpgxzgvcx6.db` 5 GB,
   `.pre-update-*.partial`, plus a stale 2.9 GB `tmpp0t78jpx.db` from Aug 20 → freed ~8 GB).
   **The git pull had already completed**, so the update was half-done at that point.
6. Re-ran `hermes update --yes --no-backup` — this time it respected the config, skipped the
   pathological full zip, rebuilt the Web UI, refreshed the model catalog, synced skills, and
   landed clean at `main @ 2179a279ae` (`✓ Code updated!`).
7. Update then blocked on the post-update gateway drain: it waits for in-flight work, and the two
   in-flight units were **this very cron job** and the `cp47-server-monitor` cron job. The drain is
   capped by `agent.restart_after_turn_timeout`, after which the gateway force-restarts.

## Verification
- `hermes --version` → **v0.21.3 (2026.9.14) · upstream 2179a279** ✅
- `git rev-parse HEAD` → `2179a279ae`, `git rev-list --count HEAD..origin/main` → **0** ✅
- working tree clean (0 modified/untracked) ✅
- `hermes doctor` → only remaining notice: "a previous `hermes update` pulled new code but did not
  restart running gateways" — expected, resolved by the drain completing / `hermes gateway restart` ⚠️

## Git workflow
- Branch `feature/hermes-update-20260915` created off updated `main`.
- **Zero tracked-file changes to commit** (the update is a fast-forward pull; the checkout is now
  identical to `origin/main`), so `git add -A` produced an empty diff.
- `git push` to the `origin` remote is **not possible** — `origin` is
  `NousResearch/hermes-agent.git` and the local credential (`openclawjohn`) gets
  `403 Permission denied`. This is upstream's repo, not ours; nothing to push.
- Branch deleted, returned to clean `main`.

## Rollback
```bash
cd /home/m/.hermes/hermes-agent
git reset --hard pre-hermes-update-20260915   # → 5eb99eb284 (v0.21.2)
```

## Lessons / actions
- **Never pass `--backup` on this install.** `state.db` is 12 GB and the full-backup SQLite
  live-copy livelocks against an active gateway. Config correctly says
  `updates.pre_update_backup: false` — leave it alone.
- Quick snapshot (`state-snapshots/20260915-010408-pre-update`) was taken and retains everything
  except the >1 GiB DBs. Code rollback is covered by the git tag above.
- Cleanup needed periodically: `/home/m/.hermes/backups/` accumulates multi-GB `tmp*.db` staging
  files when a backup aborts.
