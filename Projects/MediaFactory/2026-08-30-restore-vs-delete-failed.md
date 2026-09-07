# Media Factory — Restore V&S Queue, Delete Failed Jobs (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Problem (user report)
1. Deleting a "TEST NOW" queue made the V&S Winner Roll queue disappear.
2. A failed post in the Publishing Queue couldn't be cancelled/deleted.

## Root cause
- My leftover **TEST NOW** validation queues (Q7, Q8) were in the DB. The user deleted one to clean up; the V&S Winner Roll queue (Q4) was missing. Its 2 real posts (Bezalel 18, Boschkloof 19) and their queued jobs (J175, J192) were still healthy.
- A **failed test job** (J195, a leftover content-less test post from the broken `send_job_now`) was present, and the UI only offered Cancel for `queued` jobs — failed ones were untouchable.

## Fixes
- **Data restore:** deleted TEST NOW queues Q7/Q8; deleted the failed test job J195; recreated **V&S Winner Roll** (id 11) with Bezalel + Boschkloof, 08:00-16:00, 1/day, rotate. Original queued jobs J175/J192 preserved.
- **UI:** failed jobs now show **Retry** (Publish Now) and **Delete** (hard-remove via new `delete_job` action), so failed posts can be cleared.
- New `delete_job` action in views (removes the PublishingJob row entirely).

## Verify
- Queues: V&S Winner Roll (2 posts). Jobs: Bezalel queued, Boschkloof queued. No TEST NOW, no failed job.
- Page 200; failed jobs render Retry + Delete.

## Lessons
- My validation runs kept creating TEST NOW queues and test content. Cleaner approach needed: test via isolated DB or always clean up after.
- `send_job_now` originally broken (missing `_tz`) producing failed test jobs.

## Files
- `publishing/views.py` (delete_job)
- `templates/publishing/event.html` (failed-job Retry/Delete)
- data restore run
