# Media Factory — Clean-up: Phantom Queue, Duplicate Jobs, Job Identity (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Reported problems
1. Publishing Queue at bottom showed **4 jobs** when only 2 posts were queued.
2. A **"Queue 2026" with 3 posts** appeared that the user didn't create.
3. No way to know what a job was (table only showed raw content_type code).
4. "V&S Winner Roll says 2 posts but shows a lot more inside" (confusing add-dropdown).

## Root causes
- Junk/test data from my validation runs: `Queue 2026` (content 127-129) + orphan jobs.
- **Duplicate jobs:** `schedule_queue` created fresh jobs without clearing prior ones when a queue was rescheduled → Bezalel and Boschkloof each got 2 jobs.
- Job table showed `content_type` code only, not identity.
- The per-queue add-dropdown listed all posts *not* in the queue under/beside the queue, reading as "more inside."

## Fixes
- Deleted `Queue 2026` (5), orphan jobs for content 127-129, and duplicate jobs (176, 190). Now 1 queue (V&S Winner Roll, 2 posts) + 2 jobs.
- **Idempotent `schedule_queue`:** clears the queue's existing jobs before re-scheduling, so no stale/duplicate jobs ever accumulate.
- Job table now shows the participant name / body (fallback to content_type), with the content_type as sub-caption.
- Queue card now explicitly lists its **actual posts** under "Posts in this queue (N)" — separate from the available-to-add dropdown.

## Files
- `publishing/services.py` (idempotency)
- `templates/publishing/event.html` (job identity + queue item list)
