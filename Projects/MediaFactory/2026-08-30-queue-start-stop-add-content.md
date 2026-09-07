# Media Factory — Queue with Start/Stop Dates + Add Content (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## The model the user asked for
A **queue with start + stop dates**, then **post content into it** (rather than a one-off consume that destroys the content).

### Reworked `RotatingQueue`
- Old field `duration_days` (confusing "how long") replaced with explicit **`start_date` → `stop_date`** + `interval_days`.
- `date_range()` yields the posting days from start to stop, stepping by interval.
- `expand_rotating_queue()` schedules one item per working day across that range, **rolling over** when it runs out, skipping weekends, within the event's posting window (08:00-16:00).
- Slots are now timezone-aware (fixed naive-datetime warning).

### New "add content to queue" action
- `add_items` action: pick approved content (multiselect) on an existing queue → adds to that queue (dedupes) → re-expands to schedule the new items across the date range.
- Per-queue **"Add to queue"** picker on the Publishing page.

## Verify
- Queue: 2026-08-31 → 2026-09-04, 3 posts → Mon-Fri one per day, rolling over (post 0 repeats Thu, post 1 Fri). **Stop date honored**.
- `add_items`: 2 posts added → queue items 2 → 8 jobs generated across the range. Add-to-queue form renders.
- `manage.py check` passes; committed + pushed.

## Files
- `publishing/models.py` (RotatingQueue start/stop + date_range) + migration 0003
- `publishing/services.py` (expand_rotating_queue rework, timezone-aware)
- `publishing/views.py` (create_rotating start/stop, add_items)
- `templates/publishing/event.html` (start/stop pickers, per-queue add form)
