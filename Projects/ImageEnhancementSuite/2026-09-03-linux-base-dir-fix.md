# 2026-09-03 Linux Base Dir Fix

## Summary
Fixed critical bug where IES defaulted to Windows path `L:/Sync/AwardTools/` on Linux, causing all backgrounds and runtime directories to be unresolved.

## Problem
- `app/constants.py` had hardcoded `DEFAULT_BASE_DIR = Path("L:/Sync/AwardTools")`
- On Linux, when no `settings.yaml` exists, app tried to use non-existent Windows path
- Backgrounds existed at `~/Image_Enhancement_Suite/Image Enhancement Suite/assets/backgrounds/` but app looked in `L:/Sync/AwardTools/Image Enhancement Suite/assets/backgrounds/`
- All 7 photo background effects reported "MISSING" even though files existed

## Solution
Made `DEFAULT_BASE_DIR` platform-aware in `app/constants.py`:
```python
import os
if os.name == "nt":  # Windows
    DEFAULT_BASE_DIR = Path("L:/Sync/AwardTools")
else:  # Linux/macOS
    DEFAULT_BASE_DIR = Path.home() / "Image_Enhancement_Suite"
```

## Additional Changes
- Added `Image_Enhancement_Suite/L:/` to `.gitignore` — the Windows path directory inside repo was from symlink testing and shouldn't be tracked
- Restored profile files in `L:/` directory to pre-modification state (they got reformatted by YAML dump)

## Verification
All tests pass:
- ✅ 9/9 unit tests
- ✅ 49/49 effects render (including all 7 photo-background effects)
- ✅ Final audit: ALL PASS — GO
- ✅ Vault check: OK
- ✅ Backgrounds verified: 4 files (marble_white, rustic_wood, slate, wood_planks) resolve correctly for all 7 photo effect references

## Files Modified
- `Image_Enhancement_Suite/app/constants.py` — platform-aware default base dir
- `.gitignore` — exclude L:/ directory
- `PROJECT_STATE.md` — updated commit log

## Git
Commit: `ecd19a5` — "Fix Linux default base_dir: use ~/Image_Enhancement_Suite instead of Windows L:/ path"

## Status
**READY FOR USE** — IES now works correctly on Linux with proper default paths.
