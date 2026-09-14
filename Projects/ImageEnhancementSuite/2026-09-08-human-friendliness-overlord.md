# 2026-09-08 Human-Friendliness Pass — Overlord-Delegated

## What was done
Two human-friendliness fixes, delegated to subagents and QC'd by the overlord
(no self-coding, no self-review):

1. **Effects previews were freezing on slow machines.** `_render_sequentially`
   in `app/gui/effects_view.py` called `prepare_cutout(source, prefer_rembg=True)`
   — the slow rembg model — for every preview. The project's design rule is
   previews must use `prepare_cutout_fast` (never rembg, returns image as-is);
   only real generation uses full rembg. Changed the import + call to
   `prepare_cutout_fast`. Previews now render instantly.

2. **Terminology was inconsistent.** Nav said "Effects" but the Generate tab
   still said "Look" (label + "Choose a look." warning), and vault docstrings
   said "Looks gallery"/"look". Standardised to "Effect"/"Effects" in
   `generate.py`, `gui/vault.py`, `io/vault.py`. Left "look" untouched where it
   means "visually inspect".

## Verification (overlord QC, not self-report)
- `git diff` inspected — changes surgical, exactly as directed, no collateral.
- `pytest tests/ -q` → 9 passed (run independently).
- Headless xvfb smoke test (`tests/overlord_smoke.py`) built the real app,
  opened the demo project, and rendered **11 previews** on a real product via
  the fast cutout path — confirms the fix works end-to-end, not just imports.

## Key commit
- `4eaf0fd` Fix Effects preview speed + terminology

## Lesson
The overlord pattern works: delegate the coding, then independently verify the
diff and run the real app (not just imports). The smoke test caught nothing
broken this time, but it's the gate that prevents shipping untested GUI code.
