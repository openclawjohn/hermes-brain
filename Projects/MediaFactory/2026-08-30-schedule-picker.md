# Media Factory — Schedule Date/Time Picker for Publishing (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Problem
The Publishing "Queue Approved Content" action had no way to choose WHEN a post publishes — it used `content.scheduled_date`, which was empty for generated posts. So the user couldn't decide the publish date/time.

## Fix
Added a **"Schedule date & time"** picker (`<input type="datetime-local">`) to the bulk queue form.

- **`queue_selected`** view action now reads `scheduled_datetime` and stores it as the job's `scheduled_for` (timezone-aware).
- If left blank, it falls back to the content's `scheduled_date` (else None).

## Verification
Queued a post with `2026-09-15T10:30` → job `scheduled_for` became `2026-09-15 10:30:00+00:00`. Test item cleaned up.

## How the user schedules
Publishing → Queue Approved Content → **tick posts** → set **Schedule date & time** → **Queue Selected** → jobs appear in the queue with the chosen time → **Publish to Postiz**.

## Note
The Postiz publish call already uses `job.scheduled_for` (converted to ISO `T12:00:00Z`). So the chosen time carries through to Postiz as the scheduled post time.
