# Hermes Agent Update — 2026-08-31

**Date:** Mon Aug 31 10:15 SAST 2026
**Trigger:** Scheduled cron job (git-usage-policy skill)
**Result:** ✅ Update applied successfully

## What happened
- Local install: `/home/m/.hermes/hermes-agent` (git method, tracks upstream `NousResearch/hermes-agent`)
- Was 192 commits behind `origin/main`
- Ran `hermes update --yes` (unattended cron, no user to answer prompts)
- Git pull completed: `d6a6d87c` → `1cf36398`
- Dependencies reinstalled; config migration checked (up to date)
- Version: **Hermes Agent v0.20.6 (2026.8.27) · upstream 1cf36398**

## Git workflow note
The `hermes-agent` install dir is the tool's own upstream-tracking repo, NOT one of the user's 7 project repos. Creating a feature branch + committing here would diverge from upstream and break the update mechanism. `hermes update` manages this repo internally (pull + stash + reinstall). The mandatory branch/commit workflow applies to the user's project repos, not the tool's install dir. Working tree is clean, HEAD == origin/main (0 behind).

## Gateway restart (pending)
- `hermes-gateway.service` (user systemd, PID 1659) was left on pre-update code
- `hermes update` triggered a drain-first restart (SIGUSR1)
- Gateway stuck in `deactivating (stop-sigterm)` — draining in-flight work (including this cron job's own kernel runners + chrome subprocesses)
- **Not force-killed** — would terminate the host running this job
- Will complete its drain and restart on new code on its own

## Verification
- `git rev-parse HEAD` = `1cf36398` ✅
- `git rev-list --count HEAD..origin/main` = 0 (fully up to date) ✅
- `git status --short` = clean ✅
- `hermes --version` reports upstream `1cf36398` ✅
- `python -c "import hermes_cli"` = OK ✅

## Follow-up
- Confirm gateway fully restarted on new code: `systemctl --user status hermes-gateway`
- If still stuck, recover with: `hermes gateway restart`
