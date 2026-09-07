# 2026-09-03 Busy Indicator on Effects Tab

## Problem
User on slow machine: *"I go to Products, preview effect, and nothing happens? Why is Generate there, and what is the difference?"*

When clicking **"Preview effect →"** on a product card:
1. Takes you to Effects tab
2. Starts rendering ALL effect previews in background thread
3. **No visual feedback** while rendering (30+ seconds on slow machine)
4. Looks like the app is frozen/stuck

Also needed clarification on workflow.

## Solution
Added the **animated busy indicator** (already existed in `app/gui/busy.py`) to the Effects tab:

**Now when you click "Preview effect →":**
1. Spinner appears: **"Preparing previews…"**
2. You can SEE it's working (not frozen)
3. Previews populate one-by-one as they finish
4. Spinner disappears when all done

## Preview vs Generate — What's the Difference?

| Button | What it does | When to use |
|--------|-------------|-------------|
| **"Preview effect →"** (on product card) | Takes you to Effects tab, shows ALL effects rendered on THIS product | You want to **see what different effects look like** before committing |
| **"Generate"** (on product card) | Takes you to Generate tab | You're ready to create ALL images |
| **"Generate this effect →"** (on effect card) | Creates ALL ready products with THIS effect | You've seen previews and know which effect you want |

**Workflow:**
1. Click **"Preview effect →"** on any product
2. See all effects rendered on that product (with busy spinner)
3. Click **"Generate this effect →"** on the one you like
4. Done — all your products rendered with that effect

## Files Changed
- `app/gui/effects_view.py`:
  - Added `BusyIndicator` widget
  - Shows "Preparing previews…" while rendering
  - Hides when all previews complete

## Git
Commit: `c762fff` — "Add busy indicator to Effects tab — shows 'Preparing previews…' while rendering"

## Status
**VERIFIED** — GUI builds, busy indicator present on Effects tab.
