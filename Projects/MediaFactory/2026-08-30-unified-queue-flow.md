# Media Factory — Unified One-Queue Flow (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Why this refactor
The Publishing page had grown THREE overlapping things (a one-off "Queue Approved Content" with datetime+stagger, and a separate "Rotating Queue" with interval days) plus redundant fields ("Schedule date & time" vs "Start date"). The user correctly called this out. Fixed by unifying into ONE queue flow.

## What it is now
One **Queue** card:
- Tick approved posts
- Set **Start date** + **Stop date** (the ONLY date inputs)
- Set platform
- **Add to Queue**

The system **auto-staggers** the selected posts across the working days between start and stop: spreads them out, one-or-more per day at different times (08:00-16:00), no weekends, never past the stop date. No interval field, no manual stagger window, no "schedule date & time" override.

### `spread_queue_auto(queue)` (new)
- Distributes all a queue's items evenly over the working days in `date_range()`.
- Per-day, uses `scheduled_slots_for(..., ref_datetime=<that day>)` to stagger times within the window.
- **Bug found & fixed:** slots were anchoring to "today" instead of each actual day, causing all posts to land on the first day. Fixed by passing `ref_datetime=day_ref`.

## Existing queue actions retained
- **Add to queue** (per queue) — post more approved content into an existing queue.
- **Randomise** — shuffle the posting order.
- **Delete** — remove the queue.

## Files
- `publishing/services.py` (spread_queue_auto, anchored per-day slots)
- `publishing/views.py` (unified create_queue action; removed redundant queue/queue_selected UI paths; added all_queued to context)
- `templates/publishing/event.html` (rewritten: single Queue card + Queues list + Publishing Queue table)

## Verify
- 4 posts, start Mon 08-31 → stop Wed 09-02: Mon 08:00, Mon 12:00, Tue 08:00, Wed 08:00. All in 08-16, none past stop, no weekends.
- Page has "Add to Queue" + Start/Stop date; no "Rotating Queue"/"Queue Approved Content"/"Schedule date & time"/"interval_days"/"Stagger window" anywhere. HTTP 200.
