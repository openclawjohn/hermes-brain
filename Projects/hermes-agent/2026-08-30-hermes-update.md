# Hermes Agent Update — 2026-08-30

## What was done

Ran the scheduled Hermes Agent update and applied the latest upstream changes.

## Summary

- **Version:** v0.20.6 (2026.8.27)
- **Previous HEAD:** 4209d371 (129 commits behind origin/main)
- **Updated HEAD:** `d6a6d87c4a` — `fix(tools): restore setup_mcp's never-hand-edit instruction`
- **Commits pulled:** 129 fast-forwarded from `NousResearch/hermes-agent` origin/main
- **Version string upstream:** `e38cca50` reported by `hermes --version`
- **Model catalog** cache refreshed from checkout during update

## Update log highlights

- Desktop app rebuilt and packaged (`apps/desktop/release/linux-unpacked/Hermes`), Electron 40.10.2, `--build-only` (not launched).
- Desktop launcher entry re-installed: `/home/m/.local/share/applications/hermes.desktop`
- Bundled skills synced (1 updated: hermes-agent); all profiles up to date.
- Configuration up to date (no migration needed).

## Noted (not acted on — scheduled job, no interactive prompt)

- **Storage reclaim available:** `hermes sessions optimize-storage` — old search-index layout, frees ~5.4 GB of the 9.1 GB `state.db`. Safe to interrupt/re-run; never changes conversations. Run when convenient (foreground, needs user).

## Branch / repo

- The hermes-agent install is an upstream **read-only git clone** (origin = `NousResearch/hermes-agent`). `hermes update` pulls `main` directly; no feature branch is pushed (no write access). Working tree is clean.
- Gateway restart: deferred via drain (up to ~1995s) — completes when active cron work unit (this session) ends, then gateway restarts to load the new code.

## Verification

- `hermes --version` → `v0.20.6 (2026.8.27) · upstream e38cca50`
- `hermes --help` exits 0 (binary runs fine post-update)
- Working tree clean; HEAD at `d6a6d87c4a`
- Update log: `✓ Update complete! (v0.20.6) [main @ d6a6d87c4a]`
