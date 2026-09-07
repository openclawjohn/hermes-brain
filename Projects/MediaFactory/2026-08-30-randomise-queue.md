# Media Factory — Randomise Queue (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Feature
A **Randomise** button on each rotating queue. Clicking it:
- Shuffles the queue's item positions.
- Clears the queue's existing scheduled jobs.
- Re-expands the schedule in the new random order (one post per working day within 08:00-16:00, skipping weekends, honoring start/stop dates).

## Verify
4 items in known order → after Randomise the position order changed (e.g. 0,1,2,3 → 0,1,3,2) and jobs rescheduled. Verified via view POST (302) + re-check.

## Bug caught & fixed
The new `randomize_queue` block used local `from .models import PublishingJob` + `from .services import expand_rotating_queue`. Because Python treats any-assignment-in-function as function-local, `PublishingJob` became local for the ENTIRE `event_publishing` function, breaking the GET path (`UnboundLocalError` → 500). Both are already module-level imports, so the local imports were removed.

## Files
- `publishing/views.py` (randomize_queue action)
- `templates/publishing/event.html` (Randomise button next to Delete on each queue)
