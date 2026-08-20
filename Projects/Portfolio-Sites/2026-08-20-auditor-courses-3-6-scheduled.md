# 2026-08-20 Auditor Enrichment Courses 3-6 — Scheduled

## Date
2026-08-20

## What was done
Scheduled 4 auditor enrichment courses, each 2 hours apart, each using a distinct research technique. After each course, the auditor runs, finds issues, and fixes everything. A morning evaluation job consolidates results tomorrow.

## The 4 courses (each 2 hours apart)
| Course | Time (SAST) | Technique | Job ID |
|--------|------------|-----------|--------|
| Course 3 | 17:00 | Web Search | 5503e5efdc37 |
| Course 4 | 19:00 | Web Extract | 96c91ea4be25 |
| Course 5 | 21:00 | Browser Research | 3c52140836e6 |
| Course 6 | 23:00 | Competitor Analysis | 634a8f3a9518 |

## Morning evaluation
- **08:00 tomorrow (2026-08-21):** `Auditor Courses — Morning Evaluation` (job 984dc6b50055) consolidates all 4 courses' results into a single report, runs a fresh full audit, and writes the evaluation doc.

## Each course does
1. Research using its assigned technique (web search / web extract / browser / competitor analysis)
2. Add 3-5 NEW check functions to the auditor based on findings
3. Run the audit across all 7 sites
4. Fix EVERYTHING found (PHP-via-FTP method)
5. Re-audit to confirm 0 FAILs
6. Mandatory workflow: Obsidian doc + PROJECT_STATE/RULES update + git commit/push + skill update

## Reference file
- `/home/m/.hermes/scripts/auditor-courses-reference.md` — shared reference with credentials, fix method, and course history

## Key decisions
- Each course uses a DIFFERENT research technique to maximize new knowledge discovery
- Courses are 2 hours apart to avoid server throttling and give each time to complete
- All jobs are one-shot (repeat=1) — they run once and stop
- Morning evaluation at 08:00 gives the user a consolidated view to evaluate where things stand

## Files changed
- `/home/m/.hermes/scripts/auditor-courses-reference.md` — new shared reference
- 5 cron jobs created (4 courses + 1 evaluation)
