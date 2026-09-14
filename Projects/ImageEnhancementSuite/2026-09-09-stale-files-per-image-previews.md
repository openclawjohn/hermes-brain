# 2026-09-09 Stale files in results + per-image effect previews

## What the user reported
"I chose one effect, but it produced files for two, although it only mentioned
one in the csv. I also did not see a preview of the effect on all the images, so
I could deselect some, before saving."

## Fix 1: Results showed files for two effects
The output folder held stale `*.png` from an earlier run (different effect), and
`_show_previews` globbed ALL `*.png` in the folder — so the results panel showed
both effects even though the CSV was correct (only the current effect).
Fix: `_show_previews` now shows ONLY the files from the current report's items,
not every file in the folder.
Verified: with a stale Gallery Edition file in the folder, the previews show only
the 3 Marble files from the current run.

## Fix 2: No per-image previews before generating
The selection panel listed image IDs with checkboxes but no previews, so the
user couldn't see how each image would look with the chosen effect before
generating.
Fix: each row in the "Images to process" panel now shows a small 64px preview of
the chosen effect applied to that image, next to the checkbox.
Verified: 3 thumbnails rendered (one per Ready image).

## Verification
- 9/9 pytest green, smoke test PASS
- Stale-file scenario: previews show only current-run files
- 3 per-image effect previews rendered in selection panel

## Commit
`f6e8072` Show only current-run files in results + per-image effect previews in
selection panel
