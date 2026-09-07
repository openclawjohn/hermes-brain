# Media Factory — Post Posts Into an Existing Queue (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Problem
The user couldn't add posts to a specific queue. The top "Add to Queue" form only ever created a NEW queue — there was no way to target an existing one. Separately, each queue's inline "Add approved post" dropdown listed all posts not yet in it, which read as "queue contains everything."

## Fixes
1. Added an **"Add into existing queue (optional)"** dropdown at the top of the queue form. Tick posts → pick an existing queue → Add to Queue posts them into it (auto-approving drafts, de-duping, re-scheduling the whole queue). Leave it blank to create a new queue as before.
2. Clarified the per-queue inline dropdown label to "Add posts to \"<queue>\" (choose from posts not yet in this queue)".

## View detail
`create_queue` now resolves `existing_queue_id` → if present, `queue = existing_q` and new items are appended (de-duped), then `schedule_queue` re-schedules all items (stale jobs cleared).

⚠️ **Gotcha fixed during work:** an earlier patch anchored on shared text and accidentally corrupted the `create_rotating` block (undefined `interval`). Restored `create_rotating` from git and re-applied the change only to the unique `create_queue` block (anchored on its `frequency_unit` lines, which `create_rotating` lacks).

## Verify
- POST into existing queue with 3 fresh drafts → queue went 2→5 items, all auto-approved, 7 jobs rescheduled. OK.
- create_rotating intact (`interval` defined).
