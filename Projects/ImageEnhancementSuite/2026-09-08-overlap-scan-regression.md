# 2026-09-08 Overlap Scan Regression Fix (third user report)

## The user's complaint
"It still generated it on all three images, it did not realise the one is
overlapping, and there was no chance to export. Even worse than before."

## Root cause (found by running the REAL GUI under xvfb)
My persistence fix pointed `image_path` at the CLEANED copy (which has no
medal after removal). The overlap scan (`_scan` in app/gui/assets.py) then
scanned `rec.image_path` = the cleaned copy, found no medal, and reported
"No medals overlap" — so nothing got Skipped and all 3 images generated.

## The fix
`_scan()` now resolves and scans the ORIGINAL master via
`match_master_image(rec.image_id, masters_dir)`, falling back to
`rec.image_path` if no master resolves. The medal detection runs on the
original master, which still has the medal.

## Verification (overlord QC, REAL GUI flow under xvfb)
- Scan now flags aurora2026103 as Skipped with note "[Overlapping medal —
  excluded]".
- Generation produces only aurora202679 + aurora2026181 (2 files, not 3).
- The real exports folder has only the 2 clean images + full 14-column CSV.
- `pytest tests/ -q` → 9 passed.

## Key commit
- (commit after this note) Fix overlap scan to check original master

## Lesson
A fix that changes where `image_path` points (e.g. to a cleaned copy) can
silently break a downstream scan that reads `image_path`. When a scan/check
reports "nothing found" after a change, verify WHAT path it is reading — the
scan must check the original source, not a derived/cleaned artifact. Always
re-run the REAL GUI flow (xvfb) after any persistence or path change, not just
unit tests.
