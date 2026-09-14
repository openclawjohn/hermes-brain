# 2026-09-09 Medal-overlap false positive — image loads as Skipped on open

## What the user reported
"When I reopen the program, the first image is set to skip. Who says me opening
the program has got anything to do with medals. This is sloppy programming."

## Root cause
`medal_overlaps_product()` in `app/engine/medal_remove.py` had a false positive
on the real image `aurora2026103.jpg`. The medal's own light-gold rim/highlights
fall just OUTSIDE the gold HSV range (S just under 60), so they leaked into the
"product" mask. The ring around the medal bbox then counted those medal-edge
pixels as "product" and flagged overlap (frac 0.101, just over the 0.1
threshold) — even though the medal floats on white with clear space below the
ribbon (vision-verified).

That false positive wrote `aurora2026103: Skipped` into the real TT.ies
`record_state`. On every open, `load_and_link` restored it → the first image
loaded as Skipped. Opening the program does NOT run any scan — the skip came
purely from the persisted stale state.

## Fix
1. **Detector:** dilate the medal mask by 3px (7x7 ellipse) before excluding it
   from the product mask, so the medal's own edge pixels are never counted as
   product. Verified: aurora2026103 0.101→0.099 (FLOAT, correct); 679/6181 still
   FLOAT; a synthetic genuine-overlap case still detected (0.195). 9/9 pytest.
2. **Stale state:** removed `aurora2026103: Skipped` from the real TT.ies
   record_state. Verified via a fresh IESApp load: all 3 records Ready.

## Verification
- 9/9 pytest green
- xvfb smoke test PASS (11 previews rendered)
- Fresh IESApp load of real TT.ies → all 3 records Ready
- Genuine-overlap synthetic case still detected after the fix

## Commit
`bdfadf4` Fix medal-overlap false positive - dilate medal mask so its own rim
doesn't leak into product mask
