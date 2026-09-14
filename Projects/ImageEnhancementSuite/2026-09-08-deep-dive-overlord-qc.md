# 2026-09-08 Deep-Dive: Why IES Failed Repeatedly + Overlord QC Discipline

## The user's ask
"Please do a deep dive into everything that went wrong so many times yesterday,
and update skills, all your Overlord files to be able to catch these things
next time. It was really terrible, and frustrating, even for you."

## The failure chain (all on the real TT project)
1. **Overlap image still exported** — the batch-removal handler set every
   cleaned image back to Ready, re-including the overlap-flagged one.
2. **Worthless CSV** — Assets.csv had only 5 columns, dropping all the user's
   uploaded data (Product, Producer, Category, content, media_url).
3. **Overlap image still exported after restart** — the .ies project file
   stored no per-record status, so load_and_link reset everything to Ready.
4. **Overlap scan reported "No medals overlap"** — the persistence fix pointed
   image_path at the cleaned copy (no medal), so the scan found nothing.
5. **Medal-free images on open** — persisting the cleaned path as the display
   path showed medal-free images the user never asked for.
6. **Wrong output folder** — the user's "results" was the medal-removal
   download, not the generate output; I tested the wrong path.
7. **CSV first column** — user wants `media_url`, not `ImageID`.

## Root cause of the root cause
Every failure traced to the OVERLORD not verifying on the user's REAL data
through the REAL app flow. I tested isolated scripts and temp dirs while the
user looked at their real output folder. I mutated real project data with test
scripts. I claimed "verified" on unit tests alone.

## The fix (captured as D-004 in /home/m/DECISIONS.md + skill)
1. Test on the user's REAL data, not synthetic cases.
2. Run the REAL GUI flow (xvfb), not just headless imports.
3. Simulate the FULL lifecycle including app restart/reload.
4. Regenerate the REAL output folder + remove stale files.
5. Ask WHICH output folder the user means.
6. Never mutate the user's real project data with test scripts.
7. When a scan reports "nothing found", verify WHAT path it reads.
8. Persist the user's DECISION (status), never a derived path.
9. Every CSV carries ALL original data, first column = media_url.
10. End every finished turn with `TTTTT`.

## Files updated
- `/home/m/DECISIONS.md` — added D-004 Overlord QC Discipline.
- `image-enhancement-suite` skill — added media_url column rule + consolidated
  the overlap-scan, persistence, download, and show-original-masters lessons.
- `PROJECT_STATE.md` — documented the media_url change + deep-dive.
- Code: batch.py, generate.py, assets.py, test_engine.py — CSV first column
  is now media_url.

## Commits
- `334cb27` Replace ImageID column with media_url in all CSV outputs
- (docs commit after this note)

## Standing hierarchy
The user confirmed: "We will always keep this hierarchy of you as overlord."
The OVERLORD delegates coding to subagents, then independently QC's on real
data. The final verification is NEVER delegated.
