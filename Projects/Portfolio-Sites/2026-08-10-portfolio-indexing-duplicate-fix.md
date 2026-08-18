# Portfolio Indexing Fix — Duplicate Canonical Pages (2026-08-10)

**Project:** Portfolio of 7 SA domains

## Trigger
User received a Google email (Sat) stating saymyname.co.za "cannot be indexed" due to **"Duplicate, Google chose different canonical than user."** Asked to check all sites for fixes.

## Diagnosis
Read the actual Google emails from the user's Gmail (via Chrome Connector bridge) + the live Google Search Console indexing reports for all 7 sites. Reasons per site:

| Site | Google reason | Fixable? |
|------|---------------|----------|
| saymyname.co.za | Duplicate, Google chose different canonical | ✅ yes — `privacy-policy-2` |
| howzitza.co.za | Alternate page with proper canonical tag | ✅ yes — 3 `-2` pages |
| 5minutes.co.za | Alternate page with proper canonical tag | ✅ yes — `home-2` |
| beanel.com | Excluded by 'noindex' tag | ✅ already fine (author/search pages) |
| whippetqr.com | Excluded by 'noindex' tag | ✅ already fine |
| sumza.co.za | Blocked due to other 4xx | ✅ already fine (wp-admin) |
| zadocs.co.za | Not found (404) | ✅ already handled (system paths) |

**Root cause:** Orphaned duplicate `-2` pages serving HTTP 200 with self-canonical tags, appearing in the sitemap, competing with clean slugs:
- saymyname: `/privacy-policy-2/` (clean `/privacy-policy/` was a draft)
- howzitza: `/privacy-policy-2/`, `/personality-test-2/`, `/play-2/`
- 5minutes: `/home-2/` (front page)

## Fixes Applied (all verified live)

### howzitza.co.za
1. Deleted draft `privacy-policy` (id=3) freeing the clean slug
2. Renamed `privacy-policy-2`(320) → `privacy-policy`; `personality-test-2`(126) → `personality-test`; `play-2`(123) → `play`
3. Added `_wp_old_slug` meta so old `-2` URLs 301→ clean slugs
4. Consolidated byte-identical duplicate `/privacy/` → `/privacy-policy/` via mu-plugin (`howzitza-dup-consolidate.php`)
5. Rebuilt static sitemap (excludes `privacy`, no `-2`)

### saymyname.co.za
1. Deleted draft `privacy-policy` (id=3)
2. Renamed `privacy-policy-2`(62) → `privacy-policy`
3. mu-plugin `saymyname-privacy-redirect.php`: `/privacy-policy-2/` → `/privacy-policy/` (301)
4. Rebuilt static sitemap

### 5minutes.co.za
1. mu-plugin `5min-home-redirect.php`: `/home-2/` → `/` (front page canonical, 301) — confirmed after LiteSpeed cache purge
2. Rebuilt static sitemap excluding `home-2`

## Key Learning: LiteSpeed Cache Serves Stale 200s
The `/home-2/` redirect initially appeared NOT to fire — the 200 was a **stale LiteSpeed page cache**. Purging via `\LiteSpeed\Purge::purge_all()` + `do_action('litespeed_purge_all')` revealed the redirect was working all along. **Always purge LiteSpeed cache before verifying URL behavior changes.**

## Verification
- All 7 sites: homepage 200, sitemap 200, **0 `-2` slugs** in page sitemaps
- howzitza: `/privacy-policy/`200, `/personality-test/`200, `/play/`200, all old `-2` → 301
- saymyname: `/privacy-policy/`200, `/privacy-policy-2/`→301
- 5minutes: `/home-2/`→301 to `/`
- IndexNow pinged for fixed URLs (all 202 accepted)

## Files/Plugins Deployed
- `howzitza-dup-consolidate.php` (mu-plugin, howzitza)
- `saymyname-privacy-redirect.php` (mu-plugin, saymyname)
- `5min-home-redirect.php` (mu-plugin, 5minutes)
- Static sitemaps rebuilt on the 3 sites

## Next
- Google needs days-weeks to recrawl and drop the old `-2` URLs / update canonicals. Watch Search Console.
