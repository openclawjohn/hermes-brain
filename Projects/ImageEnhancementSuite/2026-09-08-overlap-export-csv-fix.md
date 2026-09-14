# 2026-09-08 Overlap-Export + CSV Data-Loss Fixes (user-reported)

## The user's complaint
Two things were broken on the real TT project and the user was (rightly) furious:
1. The image flagged as having an overlapping medal was STILL exported.
2. The downloaded CSV was "worthless" — it only had the new image filename and
   dropped all the data they uploaded.

## Root causes (found by running the real code, not reading intent)
1. **Overlap image exported:** `_remove_medals` in `app/gui/assets.py` set
   EVERY cleaned image back to `status="Ready"` — including overlap-flagged
   ones. The scan popup told the user to click "Remove medals in batch",
   which un-skipped the overlap image, so it got exported. Also, Skipped
   status was never persisted to the project file, so any reload reset
   everything to Ready.
2. **Worthless CSV:** `run_batch` in `app/engine/batch.py` wrote Assets.csv
   with only 5 columns (ImageID, Award, Effect, Filename, Output Folder). The
   user's original CSV had 7+ columns (Participant, Award, Product, Category,
   Sub Category, content, media_url) — all dropped.

## The fixes (delegated to subagents, QC'd by overlord)
1. `app/gui/assets.py` — overlap images (report item status == "overlap") stay
   **Skipped** after cleaning (human review — cleaned result may have lost
   product detail); only non-overlap cleaned images go Ready.
2. `app/engine/batch.py` + `app/gui/generate.py` — Assets.csv now carries ALL
   record fields: ImageID, Product, Producer, Award, Category, SubCategory,
   Variety, Vintage, Notes, media_url, content, Effect, Filename, Output Folder.
3. `tests/test_engine.py` — updated stale header assertion.

## Verification (overlord QC, on the REAL TT project data)
- Overlap scan → aurora2026103 Skipped; remove medals → it stays Skipped,
  NOT exported. Only aurora202679 + aurora2026181 are Ready/exported.
- Real generation → Assets.csv has all 14 columns with real product/producer/
  category/content preserved (e.g. "Almond and Honey Nougat", "108 Peaks",
  "Double Gold", the full congratulations content).
- `pytest tests/ -q` → 9 passed.

## Key commit
- `6779f9c` Fix overlap image export + preserve full CSV data

## Lesson
The overlord must run the feature on the user's REAL data and inspect the
actual output — not just read the code's intent or trust a subagent's
self-report. Both bugs were only visible by generating on the real TT project
and reading the actual Assets.csv + export folder.
