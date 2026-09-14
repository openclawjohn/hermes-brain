# 2026-09-09 Medal-removal skip + effects white-square + batch flow

## What the user reported
"Again after I exported images to remove medals, when I open the project again,
the file with the overlap is still set on skipped... The effects preview is
still not fixed and shows the nonsense bug white square. I want to be able to
choose all files in a project and apply an effect to them."

## Fix 1: Medal removal no longer leaves the overlap image Skipped
`_remove_medals` was setting overlap images to `Skipped` and persisting it, so
after the user exported cleaned images, the overlap file loaded as Skipped on
every open. The user's explicit action (remove medals + export) is a decision to
use the image — set it Ready. The overlap flag is informational, noted in the
notes, never blocking.

Verified via the real GUI flow (xvfb): after medal removal all 3 records stay
Ready and persist as Ready.

## Fix 2: Effects preview white-square bug
`prepare_cutout_fast` returned the source image as-is (full 1080x1080 white
background), so the preview showed a giant white square with a tiny product.
Fixed by cropping to the product's content bbox (with a small margin) before
returning. The cutout goes from 1080x1080 → 854x535, and the product fills 80%
width / 50% height of the canvas (was ~15%).

Verified: smoke test PASS, 9/9 pytest.

## Fix 3: Batch flow (verified working)
Generate renders ALL Ready images for the chosen effect, each preview has an
"include" checkbox (default checked), and "Download selected (images + CSV)"
copies only the checked images + a filtered CSV with the FULL product data.
Verified on real data: generate 3/3, untick one, export 2 images + 2-row CSV
with content intact.

## Verification
- 9/9 pytest green
- xvfb smoke test PASS
- Real GUI medal-removal flow: all 3 records Ready + persisted Ready
- Real batch flow: generate 3, untick 1, export 2 + full-data CSV

## Commits
- `b00798a` Overlap scan is informational only - never auto-skips an image
- `e5c5799` Fix effects preview white-square - crop cutout to product content
