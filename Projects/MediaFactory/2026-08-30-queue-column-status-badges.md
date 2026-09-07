# Media Factory — Queue Column + Visible Status Badges (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## User report
1. Publishing Queue didn't say which queue each post was in.
2. Status column showed nothing.

## Root cause
1. The job table had no Queue column, and no way to resolve a job to its queue.
2. The badge CSS was missing `badge--queued`/`badge--publishing`/`badge--cancelled` styles, so queued jobs rendered unstyled (effectively invisible).

## Fixes
- Added a **Queue column** to the Publishing Queue table; view now passes `job_queues` (RotatingQueueItem → queue name per content).
- Added missing badge styles so `Queued`/`Publishing`/`Cancelled` statuses are visible (amber/blue/grey).

## Verify
- Queue column shows "V&S Winner Roll" for all jobs.
- 19 badges render `badge badge--queued">Queued`.
- CSS serves `badge--queued`.
- Page 200; template tag balance OK.

## Files
- `templates/publishing/event.html`
- `publishing/views.py` (job_queues context)
- `static/css/app.css` (badge styles)
