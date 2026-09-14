# 2026-09-08 Medal-Removal Download Fix (fifth user report)

## The user's complaint
"Still absolute garbage. Still exports 3 images, and csv is garbage, unusable."

## Root cause (found by looking at the ACTUAL results folder)
The user's results folder (/home/m/Desktop/Image Enhancement results) is the
**medal-removal download**, not the generate output. It got:
1. ALL 3 cleaned images INCLUDING the overlap-flagged aurora2026103_clean.jpg
   (the "3 images").
2. cleaned_manifest.csv with only ImageID/Status/CleanedFile/Note — no product
   data (the "garbage CSV").

The download code `_offer_cleaned_download` in app/gui/assets.py copied every
item with an existing out_path (including overlap) and copied the manifest
stub CSV.

## The fix
- `_offer_cleaned_download` now skips overlap-flagged items (status !=
  'cleaned') — the overlap image is held for human review, not shipped.
- Writes `cleaned_assets.csv` with the FULL product data joined from records
  (Product, Producer, Award, Category, SubCategory, Variety, Vintage, Notes,
  media_url, content) + Status/CleanedFile/Note.

## Verification (overlord QC, on the REAL data)
- Medal removal report: aurora2026103=overlap, aurora202679=cleaned,
  aurora2026181=cleaned.
- Download now has only aurora202679_clean.jpg + aurora2026181_clean.jpg
  (2 images, NO overlap).
- cleaned_assets.csv carries full product data (e.g. "Akan Moringa, Lemongrass
  & Ginger Tea", "Alliance Foods Service", "Double Gold").
- `pytest tests/ -q` → 9 passed.

## Key commit
- `89d7dd2` Fix medal-removal download - exclude overlap image, write full-product CSV

## Lesson
The user's "results folder" was the medal-removal download, not the generate
output. I had been testing the generate path while the user was looking at the
medal-removal download. ALWAYS ask/check WHICH output the user is looking at
before assuming which code path is broken. The download must exclude
overlap-flagged images (they're for review, not shipping) and carry the full
product data, not a manifest stub.
