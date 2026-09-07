# Media Factory — Posting Window, Stagger Range, Weekend Skipping (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Posting window (event edit page)
- `Event.posting_window_start` (default 08:00), `posting_window_end` (default 16:00)
- Configurable at `/programmes/<slug>/event/<year>/edit/`

## Stagger range (Publishing page)
- Queue form now has **"Stagger window start"** and **"Stagger window end"** time pickers.
- Selected posts spread across that range. Defaults to the full window (08:00-16:00).
- Example: 2 posts, stagger 08:00-08:30 → 08:00 & 08:15.
- Even if the stagger range is set beyond the window, `clamp_to_window` forces every slot into 08:00-16:00. Verified: 07:00-17:00 range → 08:00, 10:20, 13:40 (clamped).

## Weekend skipping
- `Event.skip_weekends` (default True) + edit-page toggle.
- Any slot landing Sat/Sun pushes forward to Monday. Verified: Sat & Sun 10:00 → Mon 10:00.
- Applied in: stagger slots (via `clamp_to_window`), rotating queue slots, and the final Postiz publish send.
- **Also fixed a real bug:** `publish_postiz` was appending hardcoded `"T12:00:00.000Z"` after the scheduled time, so every post would fire at 12:00 UTC. Now sends the true scheduled time (clamped to window + weekends).

## Verify
- All within 08:00-16:00 asserted OK.
- Weekend push-forward confirmed.
- `manage.py check` passes; committed + pushed.
