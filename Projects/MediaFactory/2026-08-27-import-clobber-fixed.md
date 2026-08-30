# Media Factory — Import Data-Clobber Bug Fixed (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## The bug (root cause)
The CSV import used `update_or_create(...)` with `defaults` that **unconditionally overwrote** `website`, `facebook`, `instagram`, `hashtags`, `category`, etc.

When a re-import ran with a CSV that had **no social columns** (a different CSV format), those fields came through as empty strings and **wiped existing social data** for winners like Bezalel (website, Facebook, Instagram disappeared). The authoritative CSV had the data — the import clobbered it.

## What was restored
Repaired Bezalel Estate Cellars from the authoritative CSV (`/tmp/media_factory_import_0xo8ywg1/winners.csv`):
- website: `https://bezalel.estate/`
- facebook: `https://www.facebook.com/bezalel.estate`
- instagram: `https://www.instagram.com/bezalelestate/`
- category: `176. Brandy (from grapes) 8 Years`

## The permanent fix
Replaced `update_or_create` with `get_or_create` + a merge that **only writes non-empty incoming values**. Existing data is never clobbered by blanks on re-import. This protects socials across any CSV format.

## Verification
- Re-imported a no-social CSV against Bezalel → socials preserved (`SOCIALS PRESERVED: True`).
- `manage.py check` passes.

## Note for user
Some winners genuinely have no social data in the CSV (e.g. Bahari has no Facebook). Those stay blank and the system just won't include them.
