# Media Factory — Remove Unused Buttons + Publish Now (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Request
Remove unused buttons; add the ability to publish a post NOW.

## Changes
1. **Removed** the old per-job `Publish to Postiz` (blue) and `Mark Published` (green) buttons from the Publishing Queue table.
2. **Added per-job `Publish Now`** button → sends a single post to Postiz immediately (schedules for the current moment, ignores queue time).
3. **Added `Publish All Now`** button in the Publishing Queue card header → sends every queued job for the event immediately.
4. Kept per-job **Cancel**.
5. New services: `send_job_now(job)` (immediate send, no window clamp) in `publishing/services.py`. `publish_now`/`publish_queue_now` actions in views; `messages` toast on publish-all.

## Bugs caught & fixed during this
- The idempotency line added earlier used `content__in=items` where `items` were `RotatingQueueItem`s → would have raised ValueError on EVERY reschedule. Fixed to `content_id__in=[it.content_id ...]`.
- `send_job_now` had a missing `_tz` import (NameError) → fixed with local `from django.utils import timezone as _tz`.

## Verify (mock create_scheduled_post, no real posts)
- Direct `send_job_now`: job → published, scheduled_date = now. OK.
- View `publish_now`: status 302, job published, external set, sched = now. OK.
- View `create_queue`: 2 posts → 2 jobs across 2 weekdays. OK.
- `publish_due` cron: 0 sent (nothing due). OK.
- Button check on rendered page: only `Publish Now`(x2 for the 2 jobs), `Publish All Now`, `Cancel`; no `Publish to Postiz`/`Mark Published`.

## Files
- `publishing/services.py` (send_job_now + idempotency fix + tz fix)
- `publishing/views.py` (publish_now, publish_queue_now, messages import)
- `templates/publishing/event.html` (buttons)
