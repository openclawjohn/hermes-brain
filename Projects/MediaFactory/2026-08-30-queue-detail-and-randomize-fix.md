# Media Factory — Queue Detail Page + Compact List + Randomize Fix (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## User report
1. After adding posts + randomising V&S Winner Roll, only **1 post (Nanola)** got scheduled; the rest showed as one orphaned job at the bottom.
2. With 100+ posts possible, listing every post inline would fill the page — the list should show a count and open a detail page on click.

## Root cause (1)
`randomize_queue` called the **old `expand_rotating_queue`**, which schedules one post per day — so a single-day queue only produced 1 job. Fixed to use **`schedule_queue`** (schedules ALL items; verified 19 items → 19 jobs, all scheduled including Nanola).

## Change (2)
- **Compact Queues list** on the publishing page: shows `{{ q.item_list|length }} posts` + start/stop + frequency + platform, with the **name as a clickable link**. Removed the inline full post list (would break with huge queues).
- **New queue detail page** at `/publishing/<slug>/<year>/queue/<id>/` (view `queue_detail`, template `templates/publishing/queue_detail.html`): lists all posts with position, platform, scheduled time, and per-post job status. "Posts in this queue (19)" confirmed.

## Files
- `publishing/urls.py` (route)
- `publishing/views.py` (queue_detail view; randomize uses schedule_queue)
- `templates/publishing/event.html` (compact list)
- `templates/publishing/queue_detail.html` (new)

## Verify
- randomize → 19 jobs (all items incl Nanola). OK.
- main page: queue shows count + link, no inline dump (0 "Posts in this queue" on main).
- detail page HTTP 200, lists 19 posts with names.
