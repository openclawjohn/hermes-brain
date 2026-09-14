# 2026-09-11 Background Removal — Design (build into IES)

**Project:** Image Enhancement Suite (`/home/m/ies`)
**Task:** Turn the standalone "Background Image Remover" spec into a capability
built directly INTO IES. Design session only (no code yet this session).
**Branch to build on:** `feature/background-removal` (create from `main`).

---

## Context
The user attached `Desktop/20260911 Background Image Remover.docx` — a long,
detailed spec for a standalone Windows+Linux "Background Remover" app that would
batch-remove backgrounds from product images into transparent PNGs (recursive
folders, resume, preview, review, logs).

The user then corrected course: **"just build this functionality into the image
enhancement tool."**

## Key finding that shapes everything
IES **already contains the engine** the standalone app proposed:
- `app/engine/prep.py` → `prepare_cutout(prefer_rembg=True)` = rembg U2-Net
  matting: offline, non-destructive (in-memory copy), per-file cache, and an
  already-transparent fast path.
- U2-Net model already cached offline at `~/.rembg/models/u2net/u2net.onnx`
  (176 MB). No download, no bundling.
- Generate batch (`app/engine/batch.py`) already runs a cutout on every image.

⇒ A standalone app would duplicate a working, cached engine. Building it into IES
reuses everything.

## Decision summary
- Build a **new "Cut Out" tab** in IES: project-independent, folder→folder batch
  that writes transparent PNGs mirroring the source tree. Originals untouched.
- Reuse `prepare_cutout` (free: rembg, cache, already-transparent skip).
- New `app/engine/extract.py`: scanner + batch + resume-state + review routing +
  log/CSV.
- New `app/gui/cutout.py`: cloned Generate view worker + progress + previews.
- Wire into `app/gui/app.py` nav + views, `constants.py`.
- **Defer** (from the doc): second segmentation model, model bundling, hardware
  detection (all automatic), edge-refinement engine (high regress risk on
  crumbs/glass), background presets, multi-object split, crop+shadow.
  These are v1.1/v1.2 (matches doc roadmap) and are documented in
  `BG_REMOVAL_DESIGN.md`.

## Full design
See `/home/m/ies/Image_Enhancement_Suite/BG_REMOVAL_DESIGN.md` (kept in repo next
to `IES_BLUEPRINT.md`) for components, folder-convention reconciliation, file
list, and quality gates. This note is the Obsidian companion.

## Conventions honoured
- Git: feature branch `feature/background-removal`, commit per logical unit.
- British English (colour, cut-out). Pathlib everywhere. No hardcoded drive
  letters (Windows/Linux path-only differences), per RULES.md #6.
- Never modify master images (RULES.md #2) — in-memory copies only.
- QC: real product photos, alpha-extrema transparency check, originals
  byte-identical, one-bad-image-doesn't-stop-batch, resume test, Xvfb smoke,
  `vision_analyze` cutout check (RULES.md Quality Gates + OVERLORD/AGENTS).
- Obsidian doc + PROJECT_STATE / DESIGN_SYSTEM / RULES updated before finishing
  (AGENTS.md #9, #10).

## Next step (when approved)
✅ **BUILT (later same day).** Create branch `feature/background-removal`, build
engine, tests, GUI, wire-in, Xvfb smoke. Result:
- `app/engine/extract.py` (scanner, dedupe, batch, resume, review routing, log/CSV)
- `app/gui/cutout.py` (Cut Out tab) + wired into `app/gui/app.py`
- `tests/test_extract.py` 21 green; full suite 30/30.
- Xvfb real-GUI run on real photos passed (2 cut-outs, transparency confirmed,
  originals byte-identical).
- **Hardening pass:** atomic writes, WEBP/TIFF→valid PNG re-encode, symlink
  cycle guard, output==source rejection, output-inside-source pruning, fast
  scipy multi-object check, resume re-cuts changed masters, logs-dir ignore.
- **Edge refinement (v1.1, opt-in checkbox):** conservative halo removal
  validated on a real cutout (halo trimmed, core + detail intact).
  Launchers/requirements now install scipy for portability.
- Visual QC via `vision_analyze` screenshot: tab renders cleanly with previews.
- Deferred (v1.2+): multi-object split into separate PNGs, crop+shadow, mask
  preview. (Edge refinement from v1.1 is now built as an opt-in checkbox.)
All commits on `feature/background-removal`. Docs: `BG_REMOVAL_DESIGN.md` +
PROJECT_STATE/RULES/DESIGN_SYSTEM updated.
