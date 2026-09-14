# 2026-09-09 QC pass — intuitive "apply effect to all images"

## The user's complaint (repeated several times)
"I have asked about applying the filters to all the images several times, but
there is no intuitive functionality like that."

## QC finding
The backend (`run_batch`) already applied the effect to all Ready records and
exported the full CSV. But the UI gave ZERO visibility:
- The **Generate tab** had no image list, no checkboxes, no way to see or select
  which images would be processed. The "Generate images" button silently
  processed all Ready records.
- The **Effects tab** only previewed on ONE sample image with a per-card
  "Generate this effect" button.

So the user had no way to know what would be processed, and no way to choose a
subset. That's the "no intuitive functionality" complaint.

## Fix
Added an "Images to process" panel to the Generate tab:
- Lists every Ready image with a checkbox (all checked by default).
- Live "N of M selected" count.
- Select all / Select none buttons.
- Generate now processes only the selected images (passed through
  `IESApp.generate(..., records=selected)` → `run_batch(records, ...)`).

## Verification (overlord QC)
- Real GUI (xvfb): 3 of 3 selected by default; untick one → 2 of 3; generate
  produces exactly those 2 + full-data CSV (content intact).
- Visual confirmation: the panel renders with 3 checked images, "3 of 3
  selected", Select all/none buttons.
- 9/9 pytest green, smoke test PASS.

## Commit
`43aebef` Add intuitive image-selection panel to Generate - apply effect to all
or chosen subset
