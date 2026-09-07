# Media Factory — AI Style Split: Visual vs Writing (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `8f66151`

## What & why
The user wanted the AI style files **separated** — image/visual style in one file, language/writing style in another — so they know where to look for what. And this applies **per content type** (e.g. date reminders, winner announcements), not just one combined file.

## Structure (per event, stored as ai_style Asset records)
Each event now has **4 style assets** (2 content types × 2 style kinds):

| Content type | Visual (image) file | Writing (language) file |
|--------------|--------------------|-------------------------|
| Date reminders | `date-reminders-visual.md` | `date-reminders-writing.md` |
| Winner announcements | `winner-announcements-visual.md` | `winner-announcements-writing.md` |

- **Visual** = canvas, colours, typography, layout (what the image looks like).
- **Writing** = tone, voice, wording, structure (how the copy reads).

## What was built
- **Split** each event's combined `date-reminders.md` into `date-reminders-visual.md` + `date-reminders-writing.md` (12 assets).
- **Created** `winner-announcements-visual.md` + `winner-announcements-writing.md` per event (12 assets) — the user's example.
- **`STYLE_FILES`** now lists all 4 files (copied on clone).
- **`generate_style_file_from_identity`** rewritten to write BOTH the visual and writing files from the AI identity (colours/typography → visual; brand_personality → writing).
- **Renderer** (`create_reminder_card`) now reads `date-reminders-visual.md`.
- **Removed** the 6 redundant combined `date-reminders.md` assets.

## Verify
- `manage.py check` passes.
- 24 ai_style assets (6 events × 4 files).
- `load_style_file` returns the correct visual/writing content.
- `generate_style_file_from_identity` writes both files.
- `copy_event_style_files` copies all 4 files on clone (tested, cleaned up).

## Notes
- The programme-level + factory-level `date-reminders.md` files remain on disk as fallback defaults (when an event has no ai_style asset).
- The `editorial-guide.md` per programme is still NOT loaded by the prompt builder (writing-style gap, separate from these per-event style files).
