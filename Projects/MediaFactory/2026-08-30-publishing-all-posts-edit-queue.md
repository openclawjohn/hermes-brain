# Media Factory — Show All Posts in Publishing + Edit Queue (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Problem 1: new posts invisible in Publishing
The user added ~20 posts to the Content Library but couldn't see them in Publishing. Root cause: `publishing/views.py` filtered to `status="approved"` only, and the 20 new posts were drafts.

### Fix
- Publishing list now shows **ALL** event content (any status), so nothing is hidden.
- `create_queue` / `add_items` / legacy queue handlers no longer filter by `status="approved"` — drafts are queueable and are **auto-approved** the moment they enter a queue.

## Problem 2: no way to edit a queue
### Fix
- Added `edit_queue` action in `publishing/views.py` (before `randomize_queue`): updates name, start/stop date + time, frequency, rotation, platform; then **re-schedules the whole queue** from the new parameters (deletes stale jobs first).
- Template: added a per-queue **Edit** button that toggles an inline edit form (all params), with a **Save queue** submit.

## Verify (end-to-end test)
- Publishing rows: 20 shown (22 total − 2 already queued). ✅
- Drafts `draft → approved` auto-on-queue. ✅
- `edit_queue`: changed name/stop/freq(5/day)/platform(Instagram) → re-scheduled **10 jobs** (5×Mon 09:00–09:48, 5×Tue), all in-window, all Instagram. ✅

## Files
- `publishing/views.py` (show all, auto-approve, edit_queue)
- `templates/publishing/event.html` (Edit button + inline form)
