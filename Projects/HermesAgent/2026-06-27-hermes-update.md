# Hermes Agent Update — 2026-06-27

## What Was Done

Updated Hermes Agent from commit `19b262440` to `dbe734bef` (73 commits ahead), bringing the local installation up to date with upstream `main`.

## Key Upstream Changes

- **Social Embed Providers** — New rich embed system for YouTube, Spotify, Instagram, TikTok, Twitter/X, Vimeo, Pinterest, and Google Maps URLs in chat
- **Mermaid Diagram Rendering** — Inline Mermaid chart rendering in the desktop app
- **Zoom/Pan UI Component** — New `Zoomable` component for image/embed zooming
- **Embed Consent System** — User-facing consent prompt before loading external embeds
- **WhatsApp Cloud API** — New platform adapter for WhatsApp Business Cloud API
- **Gateway Drain Control** — Improved graceful shutdown and drain handling
- **Toolset Validation** — New CLI validation for toolset configurations
- **Desktop Enhancements** — Messaging tab, appearance settings, model settings improvements
- **New Tests** — 20+ new test files across agent, gateway, CLI, and tools
- **Documentation** — Platform support page, Nix setup guide, Termux updates

## Merge Conflict Resolved

A local uncommitted change in `apps/desktop/electron/main.cjs` conflicted with upstream. The conflict was in two areas:
1. Import style (upstream uses `const` + direct `require('./embed-referer.cjs')`; stash used `var` + `require_bootstrap_platform()`)
2. `app.on('activate')` handler (upstream added comments and `installEmbedReferer()` call)

Resolved by taking upstream version (the correct updated code).

## Git Branch

- Branch: `feature/hermes-update-20260627`
- Commit: `aa87d91ee` — "Update Hermes Agent - 20260627"
- Push to upstream NousResearch repo failed (no write access) — expected

## Verification

- `hermes --version` reports: `v0.17.0 (2026.6.19) · upstream dbe734be · local aa87d91e (+1 carried commit)`
- Status: **Up to date**
