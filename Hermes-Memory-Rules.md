---
created: 2026-06-09
tags: [hermes, memory, policy]
---

# Hermes Memory Rules

## What TO Save to Memory

✅ **User preferences** — "User prefers concise responses"
✅ **Environment facts** — "Project uses pytest with xdist"
✅ **Tool quirks** — "ftplib fails on special chars in WP Theme Editor"
✅ **Stable conventions** — "ZADocs uses Twenty Twenty-Five block theme"
✅ **Lessons learned** — "Use Python requests, not ftplib, for WP Theme Editor"

## What NOT to Save to Memory

❌ **Task progress** — "Phase 1 done", "PR #42 submitted"
❌ **Session outcomes** — "Fixed bug X", "Completed deployment"
❌ **Temporary state** — TODO lists, current work items
❌ **Stale artifacts** — PR numbers, commit SHAs, issue numbers
❌ **Anything stale in <7 days**

## Rule of Thumb

> If it will be stale in a week, it does NOT belong in memory.

## Where Things Go

- **Memory tool** — Durable facts that prevent future corrections
- **Skills** — Procedures, workflows, proven approaches
- **Obsidian** — Rich context, project documentation, reference material
- **Session search** — Task history, conversation transcripts

## Related
- [[Hermes-Config]]
- [[Visual-Verification-Rule]]
