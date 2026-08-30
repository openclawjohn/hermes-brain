# Media Factory — Overlay: Per-Event Setting + Exact Matching (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Two fixes

### 1. Overlay is now a per-event setting (default OFF)
Added `apply_award_overlay` boolean to the `Event` model (migration `0022`). Defaults to `False` for all events — so Aurora (which has no overlays) is unaffected. The overlay only composites when the event has this setting enabled.

Exposed in the **Event edit form** (`/programmes/<slug>/event/<year>/edit/`) as a checkbox: "Apply award overlay".

### 2. Fixed wrong-overlay matching
The old substring matching was wrong: award `'Gold'` matched `'2026 Award Overlay Gold & Value'` (because "gold" is a substring of "gold & value"). Rewrote `find_award_overlay()` to match by **exact token-set equality** after stripping prefixes and the year:
- `Gold` → `2026 Award Overlay Gold` ✅
- `Gold & Value` → `2026 Award Overlay Gold & Value` ✅
- `Double Gold` → `2026 Award Overlay Double Gold` ✅
- `Value` → `2026 Award Overlay Value` ✅

## Verification
- All 4 award→overlay mappings now resolve correctly.
- Event edit page shows the "Apply award overlay" checkbox.
- All events default to `apply_award_overlay=False`.

## How the user uses it
To enable overlays for an event: **Programmes & Events → <event> → Edit Event → tick "Apply award overlay" → Save**. Then generated winner content composites the correct overlay.
