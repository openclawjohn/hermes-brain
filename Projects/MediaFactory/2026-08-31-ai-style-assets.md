# Media Factory — AI Style Files as Per-Event Assets (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `309c35b`

## What & why
The user wanted a **central depository of all the AI md files** (date-reminders style, writing styles) — **one depository per event**, stored **under assets**. Previously the per-event files were scattered on the filesystem under `/srv/ai/programmes/<slug>/events/<year>/`. This session moved them into the Asset model as per-event records.

## What was built
- **Asset model extended** (`assets/models.py`, migration `0008`):
  - `asset_type` now includes `("document", "Document")`.
  - `purpose` now includes `("ai_style", "AI Style")`.
- **6 style assets created** — one `date-reminders.md` Asset record per event (purpose=`ai_style`, type=`document`), stored in the media library under the event.
- **`style_files.py` rewritten** to treat the event's Asset records as the source of truth:
  - `load_style_file` → reads the event's `ai_style` Asset first, falls back to programme/factory files on disk.
  - `copy_event_style_files` → copies asset→asset on event clone.
  - `generate_style_file_from_identity` → writes the AI-generated style file to the event's Asset record.
- **Assets page** now shows an "AI Style" purpose filter; the `date-reminders.md` asset is visible per event.

## Verify
- `manage.py check` passes.
- `load_style_file` reads from the event's Asset (V&S 2026 → correct content).
- `copy_event_style_files` copies asset→asset (aurora 2026→2028, cleaned up).
- `generate_style_file_from_identity` writes to the event's Asset (restored original after test).
- Renderer still works (generates card).
- Assets page (logged-in) shows "AI Style" filter + `date-reminders.md` asset (HTTP 200).
- Redundant filesystem per-event copies removed; programme + factory defaults kept as fallback.

## Notes
- The programme-level and factory-level `date-reminders.md` files remain on disk as fallback defaults (when an event has no ai_style asset).
- The `editorial-guide.md` per programme is still NOT loaded by the prompt builder (writing-style gap, separate from visual style).
