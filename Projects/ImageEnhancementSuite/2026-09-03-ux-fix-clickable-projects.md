# 2026-09-03 UX Fix — Clickable Projects List

## Problem
User feedback: *"If I want to open a project, I go to Projects. There I cannot click a project, and when I go to open a project, when I want to open the project clearly visible, it asks me to open a file. Not intuitive. I want to be able to open the project that I can see by clicking it."*

The old UI had:
- A list of projects (just labels, not clickable)
- An "Open" button next to each (small target, unclear)
- A separate "Open Project…" button at the top that opened a FILE DIALOG — even though projects were already listed below!

## Solution
Made the **project name itself the primary click target**:

**Before:**
```
[TT.ies                    ] [Open results…] [Open] [Delete]
```

**After:**
```
[📁 TT.ies                              ] [Show results] [Delete]
```

The entire project name is now a big clickable button with a folder emoji. Click it → project opens immediately.

Also:
- Removed the redundant "Open Project…" button (no more file dialog)
- Renamed "Open results…" to "Show results" (clearer)
- Made "Delete" button red (destructive action)
- Opening a project now takes you to "Looks" tab (visual previews)

## Files Changed
- `app/gui/projects.py`:
  - Project name → clickable button with emoji
  - Removed "Open Project…" button
  - "Open" → removed (redundant)
  - "Open results…" → "Show results"
  - "Delete" → red button
  - `_open_path()` → navigate to "Looks" tab

## User Workflow Now
1. Go to **Projects** tab
2. See your projects listed: `📁 TT.ies`, `📁 Aurora2025.ies`, etc.
3. **Click the project name** → Opens immediately, shows visual previews
4. Optional: Click "Show results" to open the exports folder

## Git
Commit: `dd7ecc6` — "Make projects clickable: project name is the button, remove redundant Open dialog"

## Status
**VERIFIED** — GUI builds successfully, project list renders correctly.
