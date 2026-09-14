# 2026-09-08 Medal Overlap Flow — Overlord Review & Fix

## What was wrong
The medal-overlap flow had dead-end steps that forced the user to redo work the software already did:

1. **Overlap scan parked images in the review queue** — after the user saw the warning and removed the medals, the image was still sitting there waiting for Approve/Skip. Pointless.
2. **Approve/Skip had no preview** — the user was deciding on something they couldn't see (the Asset Browser card showed "? REVIEW" text instead of the image).
3. **Overlap images were auto-cleaned** even though the medal sat on the artwork (removal could damage the original).

## The fix (committed)
- **Overlap scan now reports only** — it does NOT park images in the review queue. It marks flagged images **Skipped** (excluded from the batch), so they leave the queue automatically.
- **Remove medals in batch** cleans all images (including overlap) and sets them **Ready** — bringing them back into the batch.
- **Asset Browser card always shows the image** even for Needs Review / Skipped — no more "? REVIEW" placeholder, so Approve/Skip is a visible decision.
- **Review Queue shows a thumbnail** of each flagged image.
- **Generate results have per-image "include" checkboxes + one-click "Download selected (images + CSV)"** — untick the ones that don't work, download the rest + filtered CSV in one click.

## Verified (independent subagents, not self-review)
- Overlap detection: aurora2026103 → OVERLAP, 6181/679 → FLOAT (matches ground truth)
- Batch removal: all 3 images get cleaned files + manifest CSV
- GUI: all tabs build and switch cleanly
- Flow: scan → Skipped (excluded from batch) → remove → all Ready

## Key commits
- `8583b88` Fix overlap detection (ring around full medal bbox, not just gold circle)
- `09b7038` Batch flow: one-click download, overlap not auto-cleaned
- `4874cf7` Overlap images cleaned too (flag informational)
- `276d40c` Asset Browser always shows image
- `4c5556a` Overlap scan reports only, no queue parking
- `5d8146e` Overlap-flagged images Skipped (excluded from batch)

## Lesson
The overlord must check the **human flow end-to-end**, not just whether functions run. The recurring failure was shipping "it works" without walking the actual user path (flag → remove → download) and seeing the dead ends.
