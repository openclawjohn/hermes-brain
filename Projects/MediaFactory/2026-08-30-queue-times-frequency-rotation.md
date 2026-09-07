# Media Factory — Queue Times + Frequency + Rotation (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## What the user asked (and was missing)
- "No time associated with start and stop" → **Added `start_time` / `stop_time`** to the queue.
- "Enqueue 20 posts over 100 days, so many times a week, then rotate" → **Added `frequency_count` + `frequency_unit` (per day/week)** and a **`rotate`** toggle.
- "I want to post 20 posts tomorrow" → now possible.
- "I do not see how I can post fresh content to an existing queue" → the **Add approved post to this queue** form on each queue (action `add_items`) does this.

## New `RotatingQueue` fields (migration 0004)
- `start_time` / `stop_time` (TimeField, blank)
- `frequency_count` (int, default 1) + `frequency_unit` ("day"|"week")
- `rotate` (bool, default True)

## New scheduler `schedule_queue(queue)`
- Replaces `spread_queue_auto` (kept as a backwards-compat alias).
- Posts-per-day from frequency; weekly total distributed across the working days of the range.
- Queue's own `start_time`/`stop_time` bound the per-day stagger (fall back to event's 08:00-16:00).
- `rotate=True` (default) fills the whole date range, rolling over items.
- `rotate=False` only schedules items once.
- Honors posting window + skips weekends via `clamp_to_window`.

## View
- `create_queue` reads start/stop time, frequency, rotate.
- `add_items` re-schedules the queue via `schedule_queue` after adding fresh content (clears stale jobs first).

## Verify
- **20 posts tomorrow** (08-31 start=stop, 20/day, rotate): all 20 on 08-31, staggered 08:00→15:36, all in-window. OK.
- **20 posts over 100 days, rotate**, 1/day: 72 working-day jobs spanning 08-31→12-08, all in-window, no weekends. OK.

## Files
- `publishing/models.py` + migration 0004
- `publishing/services.py` (schedule_queue)
- `publishing/views.py` (create_queue, add_items)
- `templates/publishing/event.html` (time/frequency/rotate controls)
