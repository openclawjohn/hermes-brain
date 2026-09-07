# Media Factory — Automatic Publisher (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Request
"If I added the post to a queue, I would think it would publish, why would I have to do anything else?" → Queued posts must publish by themselves. Fixed.

## What was built
1. **`send_job_to_postiz(job)`** in `publishing/services.py` — reusable per-job send to Postiz (media asset resolution, real scheduled time clamped into 08:00-16:00 window). The manual `publish_postiz` button now calls this same function.
2. **`publish_due` management command** (`publishing/management/commands/publish_due.py`) — finds `status=queued` jobs whose `scheduled_for <= now` and sends them via `send_job_to_postiz`. Marks published on success; on failure increments `retry_count` (max 3) then marks `failed` with error_message.
3. **Cron every minute** (`* * * * * … python manage.py publish_due >> /tmp/mf-publish.log 2>&1`) — so a job sends automatically the moment its time arrives, no human click needed.

## Behavior now
- Add post(s) to a queue with start/stop times → system schedules them.
- When a job's scheduled time arrives, the cron publisher sends it to Postiz automatically (clamped into the posting window, skipping weekends).
- No "Publish to Postiz" click needed. The button is kept as a manual override / retry.

## Verify
- `python manage.py publish_due` runs clean: "0 sent, 0 failed" (V&S jobs scheduled for tomorrow are not due yet).
- Cron entry registered in crontab.
- migration-free (no model change).

## Files
- `publishing/services.py` (send_job_to_postiz)
- `publishing/views.py` (publish_postiz uses shared fn)
- `publishing/management/commands/publish_due.py` (+ __init__ files)
- crontab entry
