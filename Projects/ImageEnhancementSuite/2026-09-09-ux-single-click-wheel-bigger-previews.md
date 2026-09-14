# 2026-09-09 UX pass: single-click zoom, wheel scroll, bigger previews

## What the user asked
"make it so that the preview just requires a single click, and the closing of
the preview is also a single click. Also allow where there are lists of images,
for me to scroll down with the mouse, rather than clicking and dragging the
sidebar. Also make it so that there are bigger pictures, and also the ability to
have a preview in the screenshot, because that is where you eventually have to
choose, which images get the effect."

## Changes
1. **Single-click zoom** — Effects previews open the large zoom on a SINGLE
   click (was double-click), and the zoom closes on a single click too.
2. **Mouse-wheel scroll** — the "Images to process" list now scrolls with the
   mouse wheel over the list (bound recursively on the scrollable frame and its
   children), not just by dragging the scrollbar.
3. **Bigger pictures** — selection-panel preview thumbnails are now 120px (was
   64px) and the panel is taller (220px), so the user can judge the effect on
   each image before generating.

## Verification
- 3 thumbs rendered at 120px
- Wheel bound on sel_list (bind("<MouseWheel>") returns non-empty)
- Single-click zoom opens (winfo_exists=1) and closes (None)
- 9/9 pytest green, smoke test PASS

## Commit
`0d3455f` Single-click zoom, mouse-wheel scroll, bigger previews in selection panel
