# Hermes Agent Update — 2026-09-03

**Date:** Thu Sep 3 03:00 SAST 2026
**Trigger:** Scheduled cron job (git-usage-policy skill)
**Result:** ✅ Update applied successfully

## What happened
- Local install: `/home/m/.hermes/hermes-agent` (git method, tracks upstream `NousResearch/hermes-agent`)
- Was 528 commits behind `origin/main`
- Ran `hermes update` (unattended cron)
- Git pull completed: `180291162f` → `593aa74c61`
- Version: **Hermes Agent v0.21.0 (2026.8.31) · upstream 593aa74c**
- Config format migrated v39 → v40; `model_catalog.ttl_hours` replaced by `ttl_minutes` (20 default)
- Web UI rebuilt; desktop app up to date; bundled skills synced to all profiles
- `hermes-gateway` signalled to drain + restart (avoids cron-update deadlock #100179); dashboard stopped PID 1472388

## Git workflow note
The `hermes-agent` install dir is the tool's own upstream-tracking repo, NOT one of the user's 7 project repos. Creating a feature branch + committing here would diverge from upstream and break the update mechanism. `hermes update` manages this repo internally (pull + stash + reinstall). Mandatory branch/commit workflow applied to the Obsidian vault (this doc) instead. Working tree clean, HEAD == origin/main (0 behind).

## ⚠️ Toolset config warnings (post-update — needs attention)
`hermes update` reported invalid toolset references in config:
- platform `cli` → unknown toolset `messaging`
- platform `discord` → unknown toolset `messaging`
- platform `google_chat` → unknown toolset `hermes-google_chat` (has NO valid toolsets — agent will have no tools there)
- platform `teams` → unknown toolset `hermes-teams` (has NO valid toolsets)

Fix: run `hermes tools` to reconfigure the affected platforms. Known from prior cron run: `discord` not configured/enabled.

## Follow-up
- Confirm gateway fully restarted on new code: `systemctl --user status hermes-gateway`
- Fix toolset config: `hermes tools` (cli, discord, google_chat, teams)
- Optional storage reclaim (~5.9 GB): `hermes sessions optimize-storage`
