# Media Factory — Award Overlay Auto-Compositing (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## What was done
1. **Imported Vine & Spirit 2026 winners** — 23 real winners (Gold ×12, Gold & Value ×8, Double Gold ×3) with product images and social fields (website, Instagram, Facebook, hashtags).
2. **User uploaded 4 award overlays** for Vine & Spirit 2026 (stored as `award_artwork` assets):
   - Gold (`Gold.png`)
   - Double Gold (`Double_Gold.png`)
   - Gold & Value (`Gold__Value.png`)
   - Value (`Value.png`)
3. **Built auto-overlay compositing** in the content generation flow:
   - `content/services/media.py`: added `find_award_overlay()` (matches award name to overlay asset) and `composite_award_overlay()` (Pillow composite, originals never modified).
   - `content/views.py` `generate_post`: when a participant is selected, automatically composites the matching award overlay onto their product image and attaches it as the content item's `media_asset`.
   - Fixed missing `import os` in `content/views.py` (was silently swallowing the overlay error).
   - Fixed `content/detail.html` and `content/generated.html` to display the overlay (`media_asset`) instead of the raw product image.

## Verification
- **Visually confirmed** the composited image: Harmony Honeybush Gin with "DOUBLE GOLD 2026 VINE & SPIRIT AWARDS" badge in the top-right corner — legible, correctly placed, product not obscured.
- Content detail page now renders the overlay image.
- `manage.py check` passes.

## How the user uses it
1. **Content Factory → Content Type "Award Winner Announcement"** → select a participant → Generate.
2. The local AI (gemma4:26b writer + gemma4:12b checker) writes the post using the Aurora editorial template (Vine & Spirit falls back to the shared factory template).
3. The matching award overlay is auto-composited onto the product image.
4. Preview shows the post + overlay image + participant social fields (Facebook URL, Instagram, hashtags).

## Notes
- Vine & Spirit has no programme-specific content-studio, so it uses the shared `factory/content-studio/03-award-winner-announcements.md` template. The Aurora-specific template exists at `/srv/ai/programmes/aurora/content-studio/03-award-winner-announcements.md`.
- AI generation is slow (~1-2 min) because gemma4:26b is a large local model.
