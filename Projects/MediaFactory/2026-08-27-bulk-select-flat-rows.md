# Media Factory — Bulk Select / Flat-Row Views (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## What was added
Flat-row (table) views with bulk-select on the two key lists, plus a Cards/Rows toggle on each.

### Assets page (`/assets/event/<slug>/<year>/`)
- **Cards / Rows** toggle.
- **Rows view**: table with checkboxes, image thumb, title, purpose, tags, edit/delete.
- **Bulk bar**: "Announce Winners" (generates an award-winner announcement for each selected asset tied to a winning product, with the correct overlay composite, skipping ones already announced) and "Delete Selected".
- `select all` checkbox + live selection count.

### Participants page (`/knowledge/<slug>/<year>/participants/`)
- **Cards / Rows** toggle.
- **Rows view**: table with checkboxes, image thumb, name, product, award, edit.
- **Bulk bar**: "Deactivate Selected" and "Delete Selected".

## Architecture (reusable pattern)
- `assets/views.py::bulk_actions` — POST endpoint handling `announce` and `delete` on selected asset ids. Reuses `build_prompt` + `generate` + `composite_award_overlay` (single-source generation).
- `library/views.py::participants` — POST handler for `delete` / `toggle_active` on selected participant ids.
- Both templates share the same rows-view + bulk-bar + JS-select-all pattern, so any future list can adopt it the same way.

## Verification
- Assets rows view renders (checkAll, row-check, Announce/Delete, Bezalel present).
- Bulk "Announce Winners" end-to-end: selected Bezalel asset → generated announcement with Gold overlay (`2350_Gold_overlay.png`), then cleaned up.
- Participants rows view renders (pCheckAll, Deactivate/Delete, Bezalel present).
- `manage.py check` passes.

## To do next (per user)
- Extend the same rows/bulk pattern to the other lists (Content Library, Image Factory, Knowledge) — "must be for every list."
