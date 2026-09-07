# Media Factory — Fix: Status dropdown empty on milestone form (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `338bc3c`

## Problem
Could not save a date reminder for Clash 2026 at `/milestones/add/` — the bottom **Status** dropdown was empty (no options), so the form couldn't be completed.

## Root cause
`EventMilestone.approval_status` was a `CharField(max_length=20, default="proposed")` with **no `choices`**. Django renders a `Select` widget with zero options when a field has no choices → empty dropdown.

## Fix
Added `choices` to `EventMilestone.approval_status` (migration `0028`):
- `proposed` → "Needs Approval"
- `approved` → "Approved"
- `rejected` → "Rejected"

**Note:** `PostSuggestion.approval_status` (a different model) was left untouched — only `EventMilestone` was fixed.

## Verify
- `manage.py check` passes; migration applied.
- Status dropdown now renders all 3 options (default "Needs Approval").
- Full save flow tested via test client: POST creates the milestone (302 redirect), status `proposed`. Test milestone cleaned up.
