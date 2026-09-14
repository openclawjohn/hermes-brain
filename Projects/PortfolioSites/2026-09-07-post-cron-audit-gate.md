# 2026-09-07 Post-Cron Audit Gate

## What
User asked: can the Auditor check every cron job after it's done, to catch issues like the 7 bad images automatically instead of only when asked?

## What was built
Created the `post-cron-audit-gate` skill (wordpress category) that mandates an audit-fix-reaudit loop at the end of every content-writing cron:
1. Job does main work
2. Dispatch auditor subagent to verify against quality checklist
3. Fix any issues directly (PHP via FTP, fal.ai/Commons for images, theme edit for nav)
4. Re-audit until CLEAN
5. Only then report done

## Checklist enforced
- Images: 2+ per article, visually clean (no text/logo/brand/watermark/AI-gibberish), relevant, different subjects
- Text: 1500+ words, no duplicate H2s, no boilerplate
- Site health: homepage 200, sitemap 200 + count matches REST, ads.txt valid, AdSense meta=1, essential pages 200, no broken slugs, nav says "Articles"

## Wired into 3 crons
- Weekly Blog Posts - All Domains (b054e0d5026d)
- website-guardian (2df4e7130a25)
- CEO of Domains (eb66b3bea877)

Each got the `post-cron-audit-gate` skill added + a MANDATORY final audit step in the prompt. Gateway restarted to load changes.

## Why
2026-09-07: weekly blog cron published 7 articles but 7/14 images failed quality gates (text, brands, AI gibberish, irrelevant) + saymyname had "Blog" nav. Only caught because user asked. Now automatic.
