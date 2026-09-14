# 2026-09-09 Overlap scan auto-skip + batch flow verification

## What the user reported
"When I open the batch, the first image is marked as skipped, although I did not
ask for that. Next I would like to convert whole batches with an effect, choose
those it does not work with, and then export the rest."

## Root cause
The overlap scan (`_scan` in app/gui/assets.py) was setting `r.status =
"Skipped"` and persisting it via `save_record_state()`. So the first image
(aurora2026103) loaded as Skipped on every open and was excluded from
generation — even though the user never asked to skip it. The scan was
mutating state it had no business mutating.

## Fix
`_scan()` now only REPORTS which images overlap. It never changes status and
never calls `save_record_state()`. Skipping is a user decision (the card's Skip
button), not something a scan decides.

Verified via the real GUI flow (xvfb): after the scan, all 3 records stay Ready.
The scan reports the overlap but does not exclude anything.

Cleared the stale `aurora2026103: Skipped` from the real TT.ies record_state.

## Batch flow (verified working)
The user's requested flow already works end-to-end:
1. Generate tab renders ALL Ready images for the chosen effect.
2. Each preview has an "include" checkbox (default checked).
3. "Download selected (images + CSV)" copies only the checked images + a
   filtered CSV carrying the FULL product data (media_url, Product, Producer,
   Award, Category, SubCategory, Variety, Vintage, Notes, content, Effect,
   Filename, Output Folder).

Verified on real data: generate 3/3, untick one, export 2 images + 2-row CSV
with content (FB/IG/website links) intact.

## Verification
- 9/9 pytest green
- Real GUI scan flow: all 3 records stay Ready after scan
- Real batch flow: generate 3, untick 1, export 2 + full-data CSV

## Commit
`b00798a` Overlap scan is informational only - never auto-skips an image
