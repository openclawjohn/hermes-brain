---
created: 2026-06-09
tags: [hermes, policy, visual, qc]
---

# Visual Verification Rule

**Effective:** June 2026

## The Rule

> **NEVER** present improvements as done without first confirming them visually via `browser_vision`.

## What This Means

1. **Look before presenting** — Always navigate to the page and take a screenshot
2. **Fix autonomously** — If something is wrong, fix it without asking
3. **Only report when done** — Report back only when everything works AND looks right
4. **Do not guess** — Visual confirmation is mandatory

## Workflow

```
1. Make changes (code, deploy, edit)
2. browser_navigate to affected page
3. browser_vision with specific question
4. If wrong → fix → repeat from step 2
5. If right → report to user
```

## Related
- [[Hermes-Memory-Rules]]
- [[WordPress-Deployment]]
