# 2026-08-31 — V&S queued posts not publishing (full chain of blockers)

## Symptom
User queued 19 posts in Media Factory for V&S (Vine & Spirit Awards). They were not appearing on Instagram/Facebook.

## Root causes (stacked, in order discovered)
1. **Cron never ran python.** Cron line used `source venv/bin/activate && python manage.py publish_due`. Cron runs under `/bin/sh` (dash), where `source` is a bash-ism → died every minute, python never ran, log empty. **Fix:** `venv/bin/python manage.py publish_due`.
2. **Combined platform posted to neither.** Queue platform stored as `"Facebook + Instagram"`; `create_scheduled_post` only matched single platforms → no integration id. **Fix:** resolve to a LIST of integration ids, one `posts[]` entry per id.
3. **Malformed Postiz date → 400.** `isoformat() + ".000Z"` → `...T08:00:00+00:00.000Z`. **Fix:** `astimezone(datetime.timezone.utc).strftime("%Y-%m-%dT%H:%M:%S.000Z")`.
4. **Postiz orchestrator disconnected from Temporal.** Orchestrator (pm2 id 1) started when Temporal wasn't ready → connected to `null`, never retried → posts stuck in `QUEUE` forever. Restarting spawned duplicates fighting over port 3002 (`EADDRINUSE` crash-loop). **Fix:** kill stale node procs holding 3002, `pm2 stop`/`start orchestrator` → clean connect.
5. **Instagram/Facebook need a PUBLIC image URL.** Sent `172.19.0.1:8001` (private Docker IP) → "Media fetch failed"/"Invalid file". **Fix:** cloudflared quick tunnel to 8001, use public host.
6. **RGBA PNG rejected by IG/FB.** Overlay PNGs have alpha → "Invalid file" (FB error 2069019). **Fix:** flatten onto white → RGB JPEG (`_postable_image_url`).
7. **Postiz API rate limit 90/hour.** Sending 12+ at once + retries → 429. **Fix:** send spaced ≥30-40s apart.

## What was done
- Patched `publishing/services.py`: platform list, date format, `_postable_image_url` (flatten RGBA→RGB + public URL), both send functions.
- Fixed cron to `venv/bin/python`.
- Restarted Postiz orchestrator cleanly (killed stale procs, pm2 restart).
- Started cloudflared quick tunnel `https://falls-sagem-recreation-hybrid.trycloudflare.com` → 8001.
- Re-sent posts; 12/19 published. Remaining 7 hit Postiz 429 rate limit; background job `/tmp/send_remaining.sh` waits for block to clear then sends them spaced out.
- Commit `583b728`, pushed to Gitea.

## Current state
- 12/19 posts PUBLISHED (Instagram + Facebook).
- 7 remaining (jobs 271-277) queued, waiting for Postiz rate-limit block to clear (~43 min), then auto-sent by background job.

## Verification
- `venv/bin/python manage.py publish_due` → "N sent, 0 failed".
- Postiz DB shows PUBLISHED for the 12.
- Public URL reachable from postiz container (200, valid JPEG).
