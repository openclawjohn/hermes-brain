# Media Factory — Bulk Winner Announce + No Hardcoding (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## 1. Bulk winner selection in Content Factory
The Content Factory → Award Winner Announcement form now shows a **bulk checkbox list** of all winners (with a "Select all winners" toggle) when the "Award Winner Announcement" content type is chosen. Selecting multiple generates one announcement per winner, each with the correct overlay composited, then shows all on a `generated_bulk.html` results page. Single-participant mode still works for non-winner content types.

## 2. Removed Aurora hardcoding (factory-correct)
`publishing/services.py` had **Aurora-specific Postiz integration IDs hardcoded** (`INTEGRATION_IDS`), which meant publishing only worked for Aurora. Fixed:
- Added `postiz_instagram_id` and `postiz_facebook_id` to the `Programme` model (migration `0024`).
- Added them to the Programme form.
- Rewrote `publishing/services.py` to resolve integration ids **from the programme record**, not hardcode.
- `publishing/views.py` now passes `programme=event.programme` to `create_scheduled_post`.
- Migrated Aurora's existing IDs into the DB so Aurora keeps working.

## Verification
- Bulk winner generation added to generate template (23 winners listed).
- Publishing service now has 0 hardcoded Aurora IDs.
- `manage.py check` passes.

## Note for user
To publish for a new programme (e.g. V&S), you must set its **Postiz Instagram/Facebook integration ids** on the Programme edit form. The API key now comes from `POSTIZ_API_KEY` env var (was hardcoded).
