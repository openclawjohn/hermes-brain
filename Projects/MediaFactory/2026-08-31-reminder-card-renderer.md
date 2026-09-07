# Media Factory — Reminder-Card Renderer + AI Style Generation (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `f697985`

## What & why
The user clarified the date-reminder style was **created using the local AI** (not hand-written), and that when a new colour scheme is created, the local AI should regenerate the style. This session built the missing pieces:
1. A **generic reminder-card renderer** (trivia/info/milestone cards) — the deadline-graphic renderer already existed but only handled "X DAYS TO GO".
2. A **"Generate image" button** on the Date Reminders page.
3. **AI regeneration of the date-reminders style file** when a new colour scheme is approved.

## What was built
- **`create_reminder_card(event, milestone)`** in `content/services/media.py` — renders a 1080×1350 PNG card: top accent band, title (milestone name), date (with optional end-date range), wrapped description, footer. Reads `VisualProfile.resolve_for(event)` for colours/fonts + the date-reminders style file. Saves as an `Asset` (purpose="generated").
- **`milestone_generate_image` view** + URL `milestone/<id>/generate-image/` — POST generates the card, redirects back with a success/error message.
- **"Generate image" button** per milestone row on the Date Reminders page.
- **`generate_style_file_from_identity(event, identity)`** in `content/services/style_files.py` — writes the event's `date-reminders.md` from the AI visual identity (colours, typography, visual_style, design_rules, background_and_text).
- **Wired into the visual-identity approve flow** (`brand/views.py`) — when a new colour scheme is approved, the style file is regenerated from the AI identity.

## Verify
- `manage.py check` passes.
- `create_reminder_card` generates a correct 1080×1350 PNG (visually inspected: gold #d1a400 band/title, dark date, wrapped description, footer).
- `generate_style_file_from_identity` writes the event style file with the AI colours.
- HTTP flow: milestones page renders the button (200), POST generates the asset (302 redirect), asset saved.
- No test data leaked (leftover test milestone cleaned up; real generated assets untouched).

## Notes
- The renderer uses the `VisualProfile` colours/fonts (which the style file documents). The style file is currently informational for the renderer — the renderer reads the profile directly. If the user wants the renderer to parse the style file itself (e.g. per-event layout overrides), that's a follow-up.
- The `editorial-guide.md` per programme is still NOT loaded by the prompt builder (writing-style gap, separate from visual style).
