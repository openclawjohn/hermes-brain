# Media Factory — AI Loading Overlay (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## What was added
An **AI generation loading overlay** so the user knows the page isn't stuck — the local AI can take 1–2 min per post.

### Content Factory (generate form)
- Full-screen overlay with spinner + "Generating your post…" shown the instant "Generate Content" is pressed.
- **AJAX submit** (`fetch`) so the overlay stays visible for the whole AI wait instead of flashing during a normal form navigation; page body is replaced with the result HTML when done. On error, reloads.
- Both single-participant and bulk-winner modes covered.

### Assets page "Announce Winners"
- Same overlay ("Generating winner announcements…"), triggered by the `showLoading` flag on the Announce button only (not Delete).

### CSS
- `.gen-loading` overlay + `.gen-loading__spinner` (spin keyframes), added to `app.css`.

## Verification
- AJAX POST to generate returns result HTML and creates the ContentItem + composited overlay asset (tested end-to-end).
- Both Content Factory and Assets pages render the overlay markup.

## Also this session
- Set up **nightly Media Factory DB backup** (3:15am, 14-day retention, `backups/backup_media_factory_db.py`) — guard against future destructive drops.
- Cleaned orphaned generated assets (my test leftovers); user's two drafts (Bezalel, Boschkloof) confirmed safe with their overlay images.
