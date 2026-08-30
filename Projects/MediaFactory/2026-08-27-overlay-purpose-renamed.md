# Media Factory — Overlay Purpose Renamed (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## What changed
Overlays are now defined as **"Overlay"** purpose (not "Award Artwork", which is a different thing).

- `Asset.PURPOSES`: removed `award_artwork`, added `overlay`.
- Migration `0007_alter_asset_purpose`.
- Updated `content/services/media.py`, `content/views.py`, `content/services/visual_identity.py` to filter on `purpose="overlay"`.
- Migrated the 4 existing V&S 2026 overlays to `purpose="overlay"`.

## Verification
- Overlay dropdown shows all 4 overlays (Double Gold, Gold, Gold & Value, Value).
- Matching by `award_name` works, all with `purpose='overlay'`.
- `manage.py check` passes.

## How to map an overlay
1. **Assets → Upload** (or Edit an existing overlay) → **Purpose = Overlay**
2. Set **Award name** = exact award (e.g. `Gold`, `Double Gold`).

## Use/not-use overlay
Content Factory → Award Winner Announcement → **Award Overlay** dropdown:
- Pick a specific overlay → use it
- **Auto-match** → overlay follows the participant's award
- **None** → no overlay (plain product image)
