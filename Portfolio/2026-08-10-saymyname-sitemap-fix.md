# Portfolio Audit — 2026-08-10 Cron

## Status: Stable Phase F (maintenance mode)
All 7 homepages held HTTP 200 across all 4 passes. Zero oscillation, zero rebalancing.

## REST Content Counts (match Phase F baseline)
- beanel.com: 33 posts / 9 pages
- howzitza.co.za: 15 / 14
- sumza.co.za: 32 / 61
- zadocs.co.za: 66 / 9
- saymyname.co.za: 26 / 15
- whippetqr.com: 37 / 18
- 5minutes.co.za: 29 / 9

## Fix Applied: saymyname.co.za sitemap
Previous cycle flagged 2 dead `-v2` URLs in `post-sitemap.xml` (ndebele-baby-names-*-v2, tsonga-baby-names-*-v2 → 404).
Applied static XML regeneration via FTP PHP script (`saymyname_sitemap_fix.php`):
- Wrote fresh `post-sitemap.xml` with 26 live published posts (matches REST count exactly)
- Verified: 26 locs, 0 dead `-v2` slugs
- Deleted fix script from server after execution

## saymyname essentials (all healthy)
- /about/, /contact/, /contact-us/, /terms-of-service/ = 200
- /privacy-policy/, /terms/ = 301 → canonical (normal)
- /about-us/ = 404 (not used on this site, non-issue)
- ads.txt = 200, AdSense meta count = 1

## Remaining backlog
- Broken -2/-3 slugs: 5minutes 15, saymyname 0 (now cleared), whippetqr 4, beanel 2, howzitza 2, sumza 2, zadocs 0
- Rank Math sitemap exclusion gaps (REST vs sitemap) persist as config tasks (not server issues)
