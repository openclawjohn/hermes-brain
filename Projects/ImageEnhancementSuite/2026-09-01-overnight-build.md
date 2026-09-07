# Image Enhancement Suite — 2026-09-01 Overnight Build

**Project:** Image Enhancement Suite (IES)
**Date:** 1 September 2026 (overnight session)
**Path:** `/home/m/ies/Image_Enhancement_Suite/`

---

## What was built

Complete Version 1 engine for IES — a desktop app that generates professional
marketing images for award-winning food & beverage products by compositing
product photos onto programmatic marketing backgrounds.

### Verified working (tests + real image output)
| Component | Status |
|-----------|--------|
| Compositor engine (backgrounds, alpha shadows, contact shadow, reflection, vignette) | ✅ |
| Background removal via rembg (U2-Net, on-device) | ✅ clean cutouts |
| CSV import — Media Factory layout, ImageID from `media_url` | ✅ |
| Asset linking by ImageID, missing/duplicate detection | ✅ |
| Batch generator + per-effect `Assets.csv` for Media Factory | ✅ |
| Filename templates with empty-token collapsing | ✅ |
| 5 event profiles × 5–6 effects each (editable YAML data) | ✅ |
| Award badge fallback (gold medallion) when no overlay PNG | ✅ |
| Overlay detection (OpenCV SIFT) | ✅ engine, untuned on real stickers |
| Desktop GUI (Dashboard, Projects, Asset Browser, Generate) | ⚠️ imports OK, not visually verified (headless) |
| CLI (init/import/generate/effects/scan) | ✅ full pipeline verified |
| Launchers `IES.bat` (Win) + `IES.sh` (Linux) | ✅ written, not executed on target OS |

**Tests:** `python -m pytest tests/test_engine.py -q` → **9 passed**.

---

## Key decisions

- **Pillow + numpy** for compositing; **rembg** (on-device U2-Net) for
  background removal; **OpenCV (SIFT)** for overlay detection; **CustomTkinter**
  for GUI. All cross-platform (Linux now, Windows by path change).
- **Bundled profiles** live in `app/data/profiles/` and are seeded to the
  profiles folder on first init — user edits win (never overwritten).
- Profile defaults get **merged** into each effect's spec so effects stay
  concise.
- Background removal happens **in-memory on copies only** — master images and
  ZArtwork are never modified.
- Badge fallback renders a styled medallion so every image carries its award
  even before real overlay PNGs exist.
- ImageID derived from `media_url` filename (e.g. `aurora2026103`) when no
  numeric ID column exists. Permanent — never changed.

## Pitfalls hit & fixed

- **PIL can't write RGBA→JPEG** (demo fix: convert to RGB).
- **YAML colours arrive as lists** (`[60,55,48]`), PIL needs tuples — added a
  `_color()` normaliser.
- **`alpha_composite` needs RGBA canvas** — background returned RGB; convert.
- **`alpha.getbbox()` returns None** for empty alpha → IndexError; guarded.
- **rembg**: must pass PIL image + a cached session (bytes input silently fell
  back to colour-key). Model (~176 MB `u2net.onnx`) downloads once to
  `~/.rembg/models`.
- **CLI `--base` must precede the subcommand.**

## Outstanding (next session)

- [ ] Visually launch/verify the GUI (headless box can't render it).
- [ ] Wire the Awards / Effects / Review Queue **GUI editors** (engine ready,
      UI stubbed).
- [ ] Overlay detection tuning on real award stickers + background/crop health
      checks.
- [ ] **Real-photo quality pass** — render Aurora "Gallery Edition" + "Hero" on
      actual product photos user supplies.
- [ ] `rembg[cpu]` reinstall on the user's target machine (Windows) via launcher.
- [ ] Design the actual overlay PNG artwork (or source from ZArtwork).

## How to run

```bash
cd /home/m/ies/Image_Enhancement_Suite
source ../.venv/bin/activate
python -m app.cli --base /path/to/AwardTools init
python -m app.cli --base YOUR_BASE import --csv events.csv --masters photos/ --event aurora
python -m app.cli --base YOUR_BASE generate --event aurora --effect "Hero" --out exports/
# GUI:
python -m app.launcher
# or the frozen launchers IES.bat / IES.sh
```
