# 2026-09-08 Persistence Fix — Overlap Image Still Exported After Restart

## The user's complaint (second report)
"Again produced three files? The csv still has bullshit info. You said you
checked this, then run it yourself and check. You are the QC, not me."

The overlap image was STILL exported even after the previous fix. The user was
right — my earlier fix only worked within a single session.

## Root cause (found by running the real code)
The project file (.ies) stored NO per-record status. `app/io/project.py`
IESProject only saved metadata (name, event, year, csv, master_images,
output_folder, csv_columns). On every app restart, `load_and_link` rebuilt all
records from the CSV as status='Ready', so the Skipped overlap image got
exported again. The in-session Skipped status was wiped on reload.

## The fix
1. `app/io/project.py` — added `record_state` field (image_id -> {status,
   image_path}), saved in to_dict/from_dict.
2. `app/__init__.py` — added `IESApp.save_record_state()` which persists every
   record's status+image_path; `load_and_link` restores persisted state AFTER
   link_masters so a Skipped image and its chosen path win over whatever
   link_masters re-decides.
3. `app/gui/assets.py` + `app/gui/review_queue.py` — call save_record_state()
   after every status/image_path mutation (overlap scan, medal removal,
   approve/skip/locate).

## Verification (overlord QC, on the REAL TT project)
- After scan + save, the project file contains record_state for all 3 images.
- A FRESH app instance (simulating restart) keeps aurora2026103 Skipped — NOT
  exported. Only aurora202679 + aurora2026181 are Ready/exported.
- `pytest tests/ -q` → 9 passed.

## Key commit
- `73704ad` Persist per-image status across restarts

## Lesson
The overlord must test the FULL user flow including restart, not just the
in-session path. The first fix worked in-session but was wiped on reload — the
real bug was persistence, which only surfaced by simulating an app restart on
the real project data.
