# Hermes Agent Update — 2026-06-30

## What was done

Ran `hermes update` to bring the local Hermes Agent installation up to date with the latest upstream changes.

## Before

- **Version:** v0.17.0 (2026.6.19) · commit `dc5ef20d`
- **Behind:** 51 commits behind `origin/main`

## After

- **Version:** v0.17.0 (2026.6.19) · commit `f3d2dfbe`
- **Status:** Up to date with `origin/main`

## What changed (125 files, +4330/-1380)

Key changes pulled in:

- **Gateway:** Dead delivery target detection (`gateway/dead_targets.py`), improved delivery confirmation, external drain control, session race guard fixes
- **Agent:** Context breakdown module (`agent/context_breakdown.py`) for better long-context management
- **Desktop app:** Pet roaming animation (`use-pet-roam.ts`), auto-speak replies, context usage panel, voice prefs store, i18n updates (en/ja/zh/zh-hant)
- **Web app:** Redesigned theme system, profile switcher, backdrop component removed, language switcher, OAuth login modal
- **CLI:** Dashboard auth cookie handling, service manager improvements, web server updates
- **Tools:** Web tools refactored (783 lines removed), browser tool fixes, credential file updates
- **Tests:** New test suites for context breakdown, dead targets, async session DB, web tools truncation, container boot, service manager, web server, hermes state, browser CDP override
- **Infographic:** Dead delivery targets infographic added
- **Plugins:** Feishu adapter fix, WhatsApp adapter improvements, self-hosted dashboard auth provider

## Branch

- `feature/hermes-update-20260630` (local only — push to upstream repo not permitted due to permissions)
- Working tree is clean, no uncommitted changes

## Verification

- `hermes --version` reports v0.17.0 with upstream `f3d2dfbe` and "Up to date"
- `git log --oneline -1` confirms HEAD at `f3d2dfbec`
