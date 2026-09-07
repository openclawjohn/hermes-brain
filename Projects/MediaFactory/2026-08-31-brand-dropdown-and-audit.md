# Media Factory — Brand Studio dropdown fix + full menu audit (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`

## Problem
In Brand Studio at Clash, the "Upload Brand Asset" form had an **empty asset-type dropdown** even though brand assets existed. The user was frustrated at finding obvious problems one at a time and asked for a systematic audit of every menu.

## Root cause
The `BrandAssetType` table was **empty (0 rows)** — the dropdown queried `BrandAssetType.objects.filter(active=True)` and found nothing. The user's real logos live in the regular Assets (purpose=branding), but Brand Studio's own type system had never been seeded.

## Fix
Seeded 5 standard `BrandAssetType` rows (DB-only, no migration needed):
- Logo, Award Artwork, Background, Template, Banner

Verified the Brand Studio dropdown now shows all 5 options.

## Full menu audit (systematic)
Hit every main page + add-form as a logged-in user and checked for HTTP errors and empty `<select>` dropdowns:

| Page | Status | Empty selects |
|------|--------|---------------|
| Dashboard, Programmes, event, milestones, milestone-add | 200 | none |
| Assets, Image Factory, Brand Studio | 200 | none |
| Knowledge, Content Factory, Content Library, Publishing | 200 | none |
| Visual Profiles, Visual Analyse | 200 | none |
| Participants, Campaigns, Producers, Automation | 200 | none |
| Analytics, Notifications, Integrations, System | 200 | none |

**Result: no other empty dropdowns or broken pages found.** The Brand Studio dropdown was the only one.

## Notes
- The `participants` route is under `/knowledge/<slug>/<year>/participants/` (not `/participants/`).
- Document Factory routes still exist but are unlinked (per earlier decision).
