# 2026-09-03 Fix Project Flow - Products First

## Problem
Previous flow was broken:
1. Click project → jumps to "Looks" tab
2. Previews start rendering (30+ seconds on slow machine)
3. Busy indicator wasn't showing yet
4. **Looks frozen** - user thinks nothing happened

User: *"There are no products that load during products tab. Why would I go from Products tab, to looks tab."*

## Root Cause
The logic was backwards:
- **Wrong:** Open project → immediately render ALL effect previews → wait forever
- **Right:** Open project → see your products → choose to preview if you want

## Solution
Changed the flow to be intuitive:

**Now:**
1. Click `📁 TT.ies` → **Products tab** shows your 3 products immediately
2. Each product has "Preview effect →" button
3. Click it → goes to **Looks** tab → **busy spinner shows** "Preparing previews…"
4. Previews populate one-by-one
5. Click "Generate this effect →" on the one you like

**User control:** You decide when to preview, not forced.

## Files Changed
- `app/gui/projects.py`:
  - `_open_path()` → show "Products" tab (not "Looks")
  - `_create()` → show "Products" tab (not "Looks")

## Git
Commit: `9442875` — "Open project to Products tab first - user chooses when to preview"

## Status
**VERIFIED** - Products load immediately, user controls when to preview.
