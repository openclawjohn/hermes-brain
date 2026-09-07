# 2026-09-03 UX Fix — Open Project to Visual Previews

## Problem
When opening a project in IES, the app took you to the **Products** (Asset Browser) tab, which shows a list of product cards. You expected to see **visual previews** of what your images will look like immediately — not a list you have to scroll through.

User feedback: *"I opened a project, and the app disappeared. It is so difficult to see where I should go to find a product. I would think I can open a project, and then see previews."*

## Root Cause
The workflow was:
1. Open project → taken to "Products" tab (list view)
2. Have to find and click a product
3. Then navigate to "Looks" tab to see previews
4. Then maybe have to click "Use one of my images…"

This is backwards — you want to **see the rendered images first**, then decide which effect to use.

## Solution
Changed the post-open navigation in `app/gui/projects.py`:

**Before:**
```python
self.app_gui.show_tab("Asset Browser")  # Shows list of products
```

**After:**
```python
self.app_gui.show_tab("Looks")  # Shows visual effect previews immediately
```

Also improved `app/gui/effects_view.py` to **auto-pick the first ready product image** when the Looks tab is shown, so you don't have to manually select one.

## Result
Now when you open a project:
1. ✅ You see **visual previews** of all effects immediately
2. ✅ First ready product is auto-selected for preview
3. ✅ Product name is shown ("showing [Product Name]")
4. ✅ Click any effect's "Generate this effect →" button to create all images

## Files Changed
- `app/gui/projects.py` — Changed both `_create()` and `_open()` to show "Looks" tab
- `app/gui/effects_view.py` — Auto-pick first ready product and display its name

## Git
Commit: `8ec15ef` — "Open project → show Looks tab immediately with visual previews"

## Status
**VERIFIED** — GUI builds successfully, tabs load correctly, auto-preview works.
