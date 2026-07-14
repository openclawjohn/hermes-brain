# Hermes Agent Update — 2026-07-01

## What was done

Ran the scheduled Hermes Agent update check and applied the latest upstream changes.

## Summary

- **Previous version:** v0.17.0 (2026.6.19) · commit 14c4a849b (253 commits behind)
- **Updated to:** v0.17.0 (2026.6.19) · commit 8337d45c0 (up to date)
- **Commits pulled:** 253 fast-forwarded from `origin/main`
- **Files changed:** 381 files, +34,315 / -10,087

## Key changes in this update

- **Learning Graph** — New `agent/learning_graph.py`, `agent/learning_graph_render.py`, `agent/learning_mutations.py` — a persistent knowledge graph that tracks what the agent learns across sessions.
- **Verify Hooks** — New `agent/verify_hooks.py` — post-turn verification system for agent outputs.
- **Thread-Scoped Output** — New `agent/thread_scoped_output.py` — scoped output capture for subagent threads.
- **Desktop Composer Refactor** — Major rewrite of the desktop composer into modular hooks (branch, draft, drop, queue, submit, trigger, voice, etc.).
- **Desktop Starmap** — New starmap/learning-graph visualization UI (`apps/desktop/src/app/starmap/`).
- **Desktop Thread Refactor** — Thread components split into modular sub-components under `thread/`.
- **Desktop Onboarding** — New onboarding flow with provider selection.
- **TUI Journey** — New `ui-tui/src/components/journey.tsx` — interactive journey/onboarding in the TUI.
- **LSP Improvements** — New PowerShell LSP server support, reporter improvements.
- **WhatsApp Bridge** — New scripts for outbound message IDs and owner message gating.
- **Various fixes** — MoA slot resolution, Discord message overflow, Feishu channel prompts, session store stale guards, startup restart race, and many more.

## Branch

- Branch: `feature/hermes-update-20260701`
- Push failed: read-only clone (403 from GitHub) — expected for cron context
- Working tree: clean

## Verification

- `hermes --version` reports "Up to date"
- Working tree is clean, no uncommitted changes
