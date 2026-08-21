# 2026-08-21 Auditor Enrichment Courses 7-10 — Scheduled (CEO Fixes)

## Date
2026-08-21

## What was done
Scheduled 4 more auditor enrichment courses, each 2 hours apart, each using a distinct research technique. After each course, the auditor runs, finds issues, **reports to the CEO, and the CEO fixes everything**. A morning evaluation job consolidates results tomorrow.

## The 4 courses (each 2 hours apart)
| Course | Time (SAST) | Technique | Job ID |
|--------|------------|-----------|--------|
| Course 7 | 11:30 | Performance Measurement (Core Web Vitals, page weight, image formats, caching) | 0b0d71aee31f |
| Course 8 | 12:00 | Accessibility & UX (WCAG, ARIA, contrast, tap targets) | e832dcdf47e1 |
| Course 9 | 14:00 | Security & Technical SEO (TLS, mixed content, redirects, headers, consent) | 4cd1b5306553 |
| Course 10 | 16:00 | Ad Implementation & Monetization (placement, unit sizes, ad-to-content, consent) | 0a6ead1f6ec5 |

## Morning evaluation
- **08:00 tomorrow (2026-08-22):** `Auditor Courses 7-10 — Morning Evaluation` (job 03aac4acab25) consolidates all 4 courses' results, runs a fresh full audit, and **explicitly states whether there is anything left to fix**.

## Each course does
1. Research using its assigned technique
2. Add 3-5 NEW check functions to the auditor based on findings
3. Run the audit across all 7 sites
4. **Report to the CEO → CEO fixes everything** (following the CEO dispatch rules in the reference file)
5. Re-audit to confirm 0 FAILs
6. Mandatory workflow: Obsidian doc + PROJECT_STATE/RULES update + git commit/push + skill update

## Key decisions
- **CEO now does the fixing** (per user request) — each course dispatches CEO subagent(s) following the proven dispatch rules (one narrow task, pre-fetched content, exact fix method, bounded scope, independent verification, direct-work fallback)
- Each course uses a DIFFERENT research technique to maximize new knowledge discovery
- Courses are 2 hours apart to avoid server throttling
- All jobs are one-shot (repeat=1)
- Morning evaluation explicitly answers "is there anything left to fix?"

## Files changed
- `/home/m/.hermes/scripts/auditor-courses-reference.md` — updated with course history 3-6, CEO dispatch rules, and courses 7-10 plan
- 5 cron jobs created (4 courses + 1 evaluation)
