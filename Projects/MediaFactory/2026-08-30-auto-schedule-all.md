# Media Factory — Schedule ALL Posts by Default (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## The problem the user hit
To post "20 or so posts between 08:00 and 16:00 tomorrow", they'd have to compute a per-day frequency (20/day) — which is exactly the thinking they don't want to do. The frequency field was wrongly framed as required.

## The fix
`schedule_queue` now, by default, schedules **ALL of the queue's selected posts** spread across the date range — no frequency math.

- When `frequency_count <= 1` (the default "1", or unset): the queue packs every item into the available days, as many per day as fit the stagger window (08:00-16:00), staggered. With start=stop = a single day, all posts land that day, staggered across the window.
- When `frequency_count >= 2` is explicitly set (e.g. 2/day or 7/week): it acts as an **optional rotation cap** and uses the per-day/week plan + rotate+rollover.

## Frontend
- "Post every (optional)" + "Per" (Day/Week) + "Rotate" checkbox. Helper text: "Leave blank/l=1 to schedule ALL selected posts across the date range."

## Verify
20 posts, start=stop=Mon 2026-08-31, 08:00-16:00, frequency 1 (default) → **all 20 on 08-31, every 24 min: 08:00, 08:24, ... 15:36**, all in-window. No frequency math needed.

## Files
- `publishing/services.py` (schedule_queue auto-schedules all by default)
- `templates/publishing/event.html` (labels frequency as optional)
