# Media Factory — Sub Category Column Made Optional (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Problem
Import preview flagged "Missing required column: Sub Category" for a CSV that legitimately has no Sub Category column (Vine & Spirit 2026 data has no sub-categories).

## Root cause
The import view hard-required `Sub Category` as a CSV column, even though `sub_category` is an optional field on the Participant model.

## Fix
Moved `Sub Category` out of `required_columns` into a new `optional_columns` set. The missing-columns check now only flags genuinely required columns (Participant, Award, Product, Category, media_url).

## Verification
A CSV with **no Sub Category column** now previews cleanly:
- "Preview — 1 rows" ✅
- "Confirm Import" shown ✅
- No "Missing required columns" alert ✅
- Sub Category not flagged ✅
- Image "Ready" badge ✅

## Why this is a Media Factory fix (not event-specific)
The import view is shared across ALL programmes/events. Making Sub Category optional fixes it for every event — one machine, one fix. No dummy column needed.
