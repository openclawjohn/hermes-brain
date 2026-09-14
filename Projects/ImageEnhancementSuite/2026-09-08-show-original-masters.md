# 2026-09-08 Show Original Masters on Open (fourth user report)

## The user's complaint
"When I open the project, the images do not have medals anymore, even before
I check." And: "I never asked for medal free versions. I need the option."

## Root cause
`save_record_state` persisted `image_path`, which after medal removal pointed
at the CLEANED copy (no medal). On load, that cleaned path was restored, so
the app showed medal-free images. The user never asked for that — medal-free
is an OPTION, not the default. The app must always show the ORIGINAL master
(with its medal) when the project is opened.

## The fix
- `save_record_state` now persists STATUS ONLY, never image_path.
- `load_and_link` restores status only; the display path always resolves to
  the ORIGINAL master (with its medal) via link_masters.
- The user's real TT.ies was restored to status-only record_state.

## Verification
- Opening the project shows all 3 original masters (with medals).
- The overlap scan still flags aurora2026103 (OVERLAP=True).
- Generation produces only aurora202679 + aurora2026181 (2 files).
- `pytest tests/ -q` → 9 passed.

## Key commit
- `e548c38` Show original masters on open - persist status only, never cleaned image_path

## Lesson
Persisting a derived artifact path (cleaned copy) as the record's display path
silently changes what the user sees on open. Persist only the user's DECISION
(status), never a derived path. The original master is always the display
source; cleaned copies are an option the user explicitly chooses.
