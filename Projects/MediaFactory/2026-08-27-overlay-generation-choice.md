# Media Factory — Overlay: Chosen at Generation + Explicit Award Mapping (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## What changed (self-sufficient system, no hardcoding)

### 1. Overlay is now chosen at generation time (not at the event)
Removed the `apply_award_overlay` event setting (migration `0023`). The Content Factory → Award Winner Announcement form now has an **"Award Overlay" dropdown**:
- **Auto-match to award (or none)** — default; matches the participant's award to an overlay's explicit `award_name`.
- Or pick any specific overlay for that event.

### 2. Explicit award→overlay mapping (no filename/title parsing)
Added `award_name` field to the `Asset` model (migration `0006`). When uploading award artwork, the user sets the exact award name (e.g. "Gold", "Double Gold", "Gold & Value"). This is stored explicitly — **never parsed from filenames or titles**, which change every year. So the mapping is always correct regardless of naming.

- Upload form (`GeneralAssetUploadForm`) and edit form both expose `award_name`.
- `find_award_overlay()` now matches by exact normalised `award_name` equality.
- Added `list_award_overlays()` for the generation dropdown.

### 3. Backfilled existing V&S overlays
Set `award_name` on the 4 existing V&S 2026 overlays:
- Gold → "Gold", Double Gold → "Double Gold", Gold & Value → "Gold & Value", Value → "Value"

## Verification
- Generate page shows the overlay dropdown with all 4 overlays.
- **Chosen overlay**: Gold winner + Gold overlay selected → `2350_Gold_overlay.png` ✅
- **Auto-match**: Double Gold winner, no overlay chosen → `2364_Double_Gold_overlay.png` ✅
- `manage.py check` passes.

## How the user uses it
1. **Upload award artwork** → set Purpose = "Award Artwork" → set **Award name** (exact award).
2. **Content Factory → Award Winner Announcement** → pick participant → pick overlay (or Auto-match) → Generate.
3. The correct overlay composites onto the product image.
