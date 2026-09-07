# Media Factory — Posting Window + Auto-Stagger + Rotating Queue (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Three scheduling features added

### 1. Posting window (08:00–16:00 default)
- New fields on `Event`: `posting_window_start` (default 08:00), `posting_window_end` (default 16:00), `auto_stagger` (default True).
- Configurable on the **event edit page** (`/programmes/<slug>/event/<year>/edit/`).
- Posts are constrained to happen within this daily window.

### 2. Auto-stagger (posts at different times, not same time)
- When queueing selected posts **without** an explicit schedule datetime, `scheduled_slots_for()` spreads them evenly across the posting window.
- Verified: 2 posts → 08:00 & 12:00; 4 posts → 08:00, 10:00, 12:00, 14:00.
- If the user picks an explicit datetime, that wins (still snapped into the window by the postiz call).

### 3. Rotating queue (roll-over)
- New models `RotatingQueue` + `RotatingQueueItem`.
- On the Publishing page: tick approved posts, set **interval (days)**, **duration (days)**, **platform**, name optional → **Create Rotating Queue**.
- `expand_rotating_queue()` schedules one post every interval day, cycling through the items and **rolling over** to the start when the end is reached, for the full duration.
- Verified: 4 posts, 1-day interval, 10-day duration → 10 jobs, each post repeats every 4 days (post 0 at days 1/5/9).
- Rotating queues listed on the page with a **Delete** button.

## Important fix
`publishing/services.py` had a `timezone`/`datetime` import bug in the appended scheduler block — fixed (added `import datetime` and `from django.utils import timezone`).

## Files
- `programmes/models.py` + `forms.py` (Event fields)
- `programmes/migrations/0026_*` 
- `publishing/models.py` + `migrations/0002_*`
- `publishing/services.py` (scheduler + rotation)
- `publishing/views.py` (queue_selected stagger, create/delete_rotating)
- `publishing/views.py` context + `templates/publishing/event.html`
