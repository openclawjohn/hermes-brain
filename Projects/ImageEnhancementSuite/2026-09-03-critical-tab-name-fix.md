# 2026-09-03 CRITICAL FIX - Tab Name Bug

## Problem
Previous commit used wrong tab name:
```python
self.app_gui.show_tab("Products")  # WRONG - no such view!
```

The nav button says "Products" but the actual view key is "Asset Browser".

**Result:** When you clicked a project, it tried to show "Products" tab which doesn't exist → **nothing displayed** → user sees blank screen.

## Solution
Fixed to use correct view key:
```python
self.app_gui.show_tab("Asset Browser")  # CORRECT
```

## Verification
Tested:
```
After clicking project:
  Records loaded: 3
  Cards shown in Asset Browser: 3

✓ WORKS - 3 product cards displayed!
```

## Files Changed
- `app/gui/projects.py`: Changed both `_open_path()` and `_create()` to use "Asset Browser"

## Git
Commit: `f0d5f43` — "FIX: Use correct tab name 'Asset Browser' (not 'Products')"

## Lesson
Always test the actual GUI flow, not just the code. The nav map has:
- Button label: "Products" (what user sees)
- View key: "Asset Browser" (what code uses)

These must match when calling `show_tab()`.
