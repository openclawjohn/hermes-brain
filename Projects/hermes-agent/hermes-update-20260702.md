# Hermes Agent Update — 2026-07-02

## What was done

Ran `hermes update` to bring Hermes Agent from v0.17.0 (28 commits behind) to v0.18.0 (fully up to date).

## Changes pulled (30 commits total)

### Security & TLS hardening
- `agent/ssl_verify.py` — new module for TLS verification
- Honor custom CA certs on auxiliary client + harden TLS resolution
- Route Vertex AI credential/project/region through profile secret scope (fixes env leak)
- Guard browser CDP on private pages

### Bug fixes
- Lift hidden Anthropic aux output cap (MOA)
- Assert aux capability against model resolver, not frozen literal

### Features
- Discord inline bot mentions support
- Kanban watchers improvements
- New locales: Afrikaans, German, Spanish, French, Irish, Hungarian, Italian, Japanese, Korean, Portuguese, Russian, Turkish, Ukrainian, Chinese (simplified + traditional)
- Desktop app: model menu panel improvements + tests
- Codex runtime switch improvements
- Journey render improvements
- Custom provider TLS support

### Tests added
- SSL verify tests (3 new test files)
- Compression/interrupt demotion test
- Discord bot filter test
- Kanban notifier test
- Codex runtime switch test
- Journey render test
- Browser CDP tool tests
- Browser private page action guard test
- Local env blocklist test
- Slash worker ANSI test

## Branch

`feature/hermes-update-20260702` — based on `main` at `76be77009`. No local changes were needed (the update was a fast-forward merge). Push to origin was not possible due to permissions (403 on NousResearch/hermes-agent).

## Status

**Hermes Agent v0.18.0 (2026.7.1) — Up to date.**
