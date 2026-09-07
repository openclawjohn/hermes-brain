# Media Factory — Brand Studio: show real assets, drop redundant upload (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `e952719`

## Problem
Brand Studio asked the user to upload a logo / award artwork — but those were **already uploaded** in the event's regular Assets. The user asked how to navigate this.

## Root cause
`BrandAsset` (the model Brand Studio's upload form wrote to) is **dead code — nothing reads it.** The AI visual-identity analysis (`content/services/visual_identity.py`) reads the **regular `Asset`** model (purpose `branding`/`overlay`/`template`/`background`), which is where the real logos live. So Brand Studio's upload form was redundant and confusing.

## Fix
Rewrote the Brand Studio index view + template:
- **Shows the event's real brand assets** (from the regular `Asset` model, purpose branding/overlay/template/background) — the same source the AI reads.
- **Offers "Analyse with AI"** (the visual-identity analysis) for this event.
- **Shows the resolved VisualProfile** (or "no profile yet").
- **"Manage Assets"** link to the event's Assets page.
- **Removed the redundant upload form.**

## Verify
- Brand Studio (Clash 2026) shows the real Clash logo, the Analyse-with-AI button, the profile badge, and a Manage Assets link. No upload form. HTTP 200.
- `manage.py check` passes.

## Notes
- The `BrandAsset` model + its form/views still exist but are now unused by the index page. They can be removed later if confirmed dead.
- The AI analysis page (`/brand/visual-profiles/analyse/`) still requires picking the event from a dropdown — a future improvement would pre-scope it to the current event.
