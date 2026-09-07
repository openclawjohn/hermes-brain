# Media Factory — Show Queue Parameters in List (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Request
When a queue is shown in the list, show its parameters too.

## Change
`templates/publishing/event.html` — the queue list caption now shows:
- Name + post count
- Start date + start time → stop date + stop time
- Frequency (posts per day / per week)
- Rotate on/off
- Platform

Verified rendering on `/publishing/vine-and-spirit-awards/2026/`.

## Note
The existing queue "V&S Winner Roll" carries `start_time=08:00`, `stop_time=16:01` — matches what was stored. Display faithfully shows stored params.
