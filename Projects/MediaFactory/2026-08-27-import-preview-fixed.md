# Media Factory — Import Preview Fixed (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Problem
"Preview Import" button did nothing — no preview appeared after clicking.

## Root cause (two layers)
1. The redesigned `participant_import.html` template **dropped the preview-results rendering block** entirely — it only showed the upload form.
2. When I restored it, I used **wrong variable names**. The actual view builds `preview` with keys `row_count`, `row.participant`, `row.image_status` (values "Ready"/"Not found"/"No image"), plus `missing_columns`, `missing_filenames`, `missing_products`. My first attempt used `total_rows`, `row.name`, `row.image_matched` — which don't exist, so the preview silently rendered nothing.

## Fix
Rewrote the template to match the view's real data structure:
- Preview card with `preview.row_count`
- Missing-columns alert
- Matched / missing image badges
- Preview table (Participant, Award, Product, Category, Image status badge)
- Missing-filenames and missing-products alerts
- "Confirm Import" button (posts `action=import`)
- Import Result card (created/updated/images + problems)

## Verification (end-to-end, via Django test client)
- Preview POST → renders "Preview — 2 rows", "Confirm Import", "1 images matched", "1 images missing", Ready + Not found badges, missing filename listed ✅
- Confirm import → creates the Participant with its image asset ✅
- Cleaned up the test participant afterward.

## Why this is a Media Factory fix (not event-specific)
The template is shared across ALL programmes/events. Fixing it fixes the import flow for every event (Aurora, Gold Wine, SAFBA, Vine & Spirit, Clash of the Cultivars) — one machine, one fix.
