# Media Factory — Dynamic Asset Types (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `4382630`

## What & why
The user wanted to upload **award artwork** in higher resolution, but there was no "Award Artwork" category — only "Overlay". They also want **maximum flexibility**: to add their own asset types, and later specify how artwork is positioned. The asset purposes were **hardcoded** in the model, so neither was possible.

## What was built
- **`AssetType` model** (`assets/models.py`, migration `0009`) — a user-definable asset category (name, code, sort_order, active). `Asset.purpose` is now a free-form CharField storing the `AssetType.code` (no hardcoded choices).
- **Seeded 9 AssetTypes**: Branding, **Award Artwork** (new, distinct from Overlay), Overlay, Template, Background, Product Image, Generated, AI Style, Other.
- **Upload form + filter** now read from `AssetType` dynamically — so "Award Artwork" appears in the dropdown and as a filter.
- **"Manage Asset Types" page** (`/assets/types/`) — add, activate/deactivate, delete your own categories. Linked from the event Assets page.

## Verify
- Upload form shows Award Artwork + all types; filter shows them too.
- Adding a new type via the manage page immediately appears in the upload dropdown (tested with "Certificate", cleaned up).
- `manage.py check` passes.

## Notes
- Existing assets keep their `purpose` values (they match the seeded codes), so nothing breaks.
- The user can now add a type like "Award Artwork" and later extend it with positioning rules — the type system is the hook for that.
- The `award_name` field on the upload form still applies to award artwork (used to match overlays to winners).
