# Media Factory — Date-Reminder Style Files + Auto-Clone (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `1f0373a`

## What & why
The user wanted the AI style files (fonts/colours/layout for visually rendering date reminders) recreated — they were lost before this agent took over. And critically: **when a new event is created it should automatically get the files from the previous year; when a new programme is created it should get them from the one it's cloned from** — so everything starts from a good base.

## Style files created (12 total)
Resolution order: **event → programme → factory default**.
- **Factory default:** `/srv/ai/factory/date-reminders.md`
- **5 programme defaults:** `/srv/ai/programmes/<slug>/date-reminders.md` (Gold #c6a05b/Lato, Aurora #0a234b/Cardo, SAFB #5f7d8a/Atkinson, V&S #d1a400/Astra, Clash #cba742/Astra)
- **6 per-event copies:** `/srv/ai/programmes/<slug>/events/<year>/date-reminders.md`

Each file defines: canvas size (1080×1350), colours (primary/secondary/background/text), typography (heading/body/bold fonts), layout (top band, icon, title, date, body, footer), and tone.

## Auto-copy logic (new)
New module `content/services/style_files.py`:
- `copy_event_style_files(source_event, target_event)` — copies `date-reminders.md` (and any future style files in `STYLE_FILES`) from source event's dir to target event's dir.
- `copy_programme_style_files(source_programme, target_programme)` — same for programmes.

**Wired into:**
1. **`event_clone`** (`programmes/views.py`) — after creating the new event, copies the source event's style files. So cloning Aurora 2026 → 2027 gives 2027 the 2026 style files.
2. **Dashboard `create_programme`** (`dashboard/views.py`) — new "Clone style from (optional)" dropdown on the New Programme form. Picks a source programme; copies its style files to the new programme. Blank = start fresh from factory default.

## Verify
- `manage.py check` passes.
- Tested both copy paths via shell: event clone copies `date-reminders.md` to target; programme clone copies it too. Test artefacts cleaned up.
- Dashboard renders the "Clone style from" dropdown; event_clone page renders.

## Still open (next session)
- **Reminder-image renderer now works** — `create_deadline_graphic` (content/services/media.py) was already wired into content generation but used `getattr(event, "visual_profile", None)` which was ALWAYS None. Fixed to use `VisualProfile.resolve_for(event)` + `load_style_file(...)` (event → programme → factory). Verified end-to-end: generates a correct 1080×1350 PNG with the V&S logo, gold #d1a400 headline, legible text. Visually inspected.
- The `editorial-guide.md` per programme is NOT loaded by the prompt builder (writing-style gap).
