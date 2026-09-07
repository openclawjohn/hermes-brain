# Media Factory — De-dupe: Remove Redundant Add Dropdown + Test Content (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## User questions
1. Why an awkward per-queue "add posts" dropdown when the top form is easier?
2. Why 3 "Approved / general" posts with no title at the bottom?
3. Should posts be approved in advance when most say Draft?

## Answers / fixes
1. Removed the redundant per-queue `add_items` dropdown entirely. The top queue form (tick posts → pick an existing queue → Add to Queue) is the single, clear way to add posts to any queue.
2. Those 3 "general / Approved" posts (ids 127,128,129) were **leftover test content from my validation runs** (bodies 'new 0/1/2'). Deleted them. Now zero participant-less posts remain.
3. Draft is fine — **no manual approving needed**. Posts auto-approve the moment they are added to a queue. The user's 17 real posts (Gravel Jet, Harmony, Joubert, etc.) stay Draft until queued, then flip to Approved automatically.

## Files
- `templates/publishing/event.html` (removed per-queue add dropdown ~19 lines)
- data cleanup of content 127-129
