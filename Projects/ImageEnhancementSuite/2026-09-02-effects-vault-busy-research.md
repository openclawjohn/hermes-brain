# IES — Effects Vault, Busy Indicator & Effect Research (2026-09-02)

## What was done (three requests)

1. **Busy/transition feedback** — Added `app/gui/busy.py` (`BusyIndicator`), an
   animated spinner + message shown during Generate and effect-preview renders.
   Moved effect previews off the UI thread into a background renderer with ONE
   shared cutout (huge speedup + no freeze). Removable later on a fast computer.

2. **Effects Vault** — New left-nav item showing every effect per competition
   with per-event on/off checkboxes. Persisted to `vault.yaml` (new
   `app/io/vault.py`). The Effects gallery and Generate respect enabled state.

3. **Effect research** — Tested procedural marble/slate/sheen in
   `compositor.py`. **Finding: they read as digital noise, not stone — verified
   by vision analysis. Not wired into profiles.** The correct architectural fix:
   real CC backdrop photographs. Added `kind: photo` backgrounds loaded from
   `assets/backgrounds/` (aspect-fit-crop, graceful fallback), threaded through
   `render_product_image` → `run_batch` → app. Added `specular` (vertical glass
   reflection) and `floor_reflection` (marble floor) passes, both verified good
   by vision. Two new Aurora effects created; 44 effects total.

## Key decision
**Procedural textures were rejected after visual QA** — they looked like noise.
Real photography in the backgrounds folder is the path to premium. The premium
photos still need sourcing (user side).

## Files
- `app/gui/busy.py` (new), `app/io/vault.py` (new)
- `app/gui/effects_view.py`, `app/gui/generate.py` (threaded + busy)
- `app/gui/app.py`, `app/gui/vault.py` (new nav + manager)
- `app/engine/compositor.py` (photo bg, marble/slate, `imageEnhance`), `passes.py`
- `app/__init__.py`, `app/engine/batch.py`
- `app/data/profiles/aurora.yaml`

## Status
Tests green (9/9), render-all green (44/44). Commits e79cca3, e2db02e.
