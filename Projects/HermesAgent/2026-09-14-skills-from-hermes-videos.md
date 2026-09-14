# Skills Evaluated & Installed from Hermes YouTube Videos

**Date:** 2026-09-14

**Status:** Done

## What happened

User asked me to review two YouTube videos promoting Hermes Agent skills, and install whatever was genuinely useful — no duplicates, nothing unnecessary.

## Method

1. Pulled auto-captions with `yt-dlp` for both videos and read the full transcripts.
2. Listed the skills each video pitched, mapped each against the ~66 already-installed skills, and judged net-new value for this user's actual stack (7 WP sites, Media Factory Django, IES, LooLocator).
3. Cloned candidate repos, inspected their real structures, and installed only what was genuinely net-new with a clean, safe install path.

## Videos reviewed

- **V1:** "10 Hermes Agent Skills You Need to Install Today" — Brandon Builds
- **V2:** "Hermes Agent skills that make it better" — AI Labs

## Skills pitched across both videos

| Skill | Verdict | Why |
|---|---|---|
| Agent Reach | ⏭ Skip | Redundant + OpenClaw-oriented. Needs user's browser cookies for gated channels; dumps to `~/.openclaw/skills/`. Hermes already has native web_search/web_extract/browser + installed reddit-reading, rss-feeds, blocked-page-recovery, yt-dlp, gh. |
| Humanizer | Already have | `human-prose-quality` installed. |
| The Fuddle | ⏭ Skip | Token-saver; Hermes already truncates command/term output. |
| Caveman | ⏭ Skip | Token-saver; redundant with Hermes native output truncation. |
| PDF Creator | ⏭ Skip | Niche, not in workflow. |
| Oh My Hermes (OMH) | Already have | Full `omh-*` suite installed. |
| Addy Osmani Agent Skills | ✅ Installed (11) | Genuine net-new engineering skills, clean copy. |
| Anthropic Cybersecurity | ⏭ Skip | Overlaps `security-and-hardening` (installed from Addy set). |
| Codebase Memory MCP | ⏭ Hold | MCP server, heavy config; deferred — not installed now. |
| Make Interfaces Feel Better | Already have | Installed. |
| Planning with Files (V2) | Already have | Covered by dynamic-workflow / omh-ralph / todo_list. |
| Delegate Setup (V2) | Already have | Covered by code-delegation skill + built-in delegate_task. |
| RTK (V2) | Already have | Hermes native output truncation. |
| Mantis (Google) (V2) | ⏭ Skip | README: run ONLY in isolated/restricted envs; this machine holds all prod WP creds. Also needs gcloud ADC + python ADK. Redundant w/ security-and-hardening. |
| Plural (V2) | ⏭ Skip | Sponsor ad. |

## Installed — Addy Osmani engineering suite (11 skills)

Source: `github.com/addyosmani/agent-skills` (Google-backed, ~90k stars).
Copied into `~/.hermes/skills/software-development/<name>/SKILL.md`, registered & enabled:

- api-and-interface-design
- code-review-and-quality
- debugging-and-error-recovery
- incremental-implementation
- observability-and-instrumentation
- performance-optimization
- security-and-hardening
- shipping-and-launch
- source-driven-development
- spec-driven-development
- test-driven-development

Not installed from the same suite (deemed duplicate/unnecessary for this stack): git-workflow-and-versioning (have git-usage-policy), frontend-ui-engineering (have make-interfaces-feel-better), planning-and-task-breakdown (have omh/dynamic-workflow), context-engineering, ci-cd-and-automation, documentation-and-adrs, deprecation-and-migration, doubt-driven-development, constraint-driven-development, code-simplification, idea-refine, interview-me, browser-testing-with-devtools, using-agent-skills.

## Taps added

`hermes skills tap add` for Panniantong/Agent-Reach, addyosmani/agent-skills, google/mantis (left in place as version sources).

## Decision notes

- Agent Reach was the #1 most-pushed skill in both videos, but it solves an OpenClaw problem (gated-site access via cookies). Hermes solves the same access with native search/extract + yt-dlp + gh + dedicated reading skills, without user cookie-handling. Skipped as redundant.
- Mantis was skipped for safety: Google's own README forbids running it on machines with production access; this machine holds all production credentials. Static, safe `security-and-hardening` covers the audit need.

## Left (not done this session)

- Codebase Memory MCP — worthwhile later for Media Factory/IES, but it's an MCP server needing config.yaml setup + a knowledge-graph build, not a drop-in skill. Deferred.

## Files

- None created outside the skills dir. Temp clones removed.
