# Media Factory — Visual Profiles seeded + resolver (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `0a248c9`

## What & why
The user asked for the proper colours/fonts for each of the 5 award programmes, to drive per-event visual style (date reminders, trivia, etc.). The `VisualProfile` model already existed (brand/models.py) with full CRUD but was **empty (0 profiles)** and **nothing resolved it**. This session seeded it and added the resolution logic.

## Brand data extracted (live sites, CSS + logo/award artwork visually inspected)
| Programme | Primary | Secondary | Background | Fonts |
|-----------|---------|-----------|------------|-------|
| Gold Wine Awards | `#c6a05b` | `#aa8138` | `#ffffff` | Lato |
| Aurora Intl Taste Challenge | `#0a234b` (navy) | `#c5a059` (gold) | `#ffffff` | Cardo / Atkinson Hyperlegible / Inter |
| SA Food & Beverage | `#5f7d8a` (teal) | `#c5a059` (gold) | `#ffffff` | Atkinson Hyperlegible |
| Vine & Spirit | `#d1a400` | `#2e2e2e` | `#ffffff` | Astra |
| Clash of the Cultivars | `#cba742` | `#56b259` (green) | `#ffffff` | Astra |

**Note:** the user's URL `clashofthecutivars.com` is a typo (NXDOMAIN). Live site is `clashofthecultivars.com`.

## What was built
1. **Seeded 5 programme-level VisualProfiles** (per-programme default layer) — idempotent `get_or_create` by programme.
2. **Seeded 6 event-level VisualProfiles** (per-event override layer) for all existing events.
3. **Added `VisualProfile.resolve_for(event)`** classmethod — resolution order: event's own active profile → programme's active default → None. Verified both paths (event wins; falls back to programme when event profile inactive).

## Scope decision (user-endorsed)
**Both — per-event override on top of a per-programme default.** The `VisualProfile` model already supported this (both `programme` and `event` FKs).

## Verify
- `manage.py check` passes.
- `resolve_for` returns event profile for all 6 events; falls back to programme when event profile off.
- `/brand/visual-profiles/` renders all 11 profiles (HTTP 200).

## Still open (next session)
- **Date-reminder style md files** (fonts/colours/layout for visually rendering reminders) are GONE — not recoverable from git/server/vault. Need recreating.
- **No reminder-image renderer exists** — reminders are text-only `EventMilestone` rows. The style files + VisualProfile are dead until a renderer consumes them.
- The `editorial-guide.md` per programme is NOT loaded by the prompt builder (writing style gap).
