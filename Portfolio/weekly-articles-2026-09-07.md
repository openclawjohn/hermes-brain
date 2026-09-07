# Weekly Articles — 2026-09-07 (Tuesday 02:00 cron)

## Summary
Published 1 new article on each of the 7 portfolio sites. All articles: 1,500+ words, 2 different Wikimedia Commons real photos (featured at top + in-content at ~80-92% through), assigned to real categories, verified HTTP 200, and added to regenerated static sitemaps.

## Articles Published

| Site | Title | Slug | Words | Category | URL |
|------|-------|------|-------|----------|-----|
| beanel.com | What Is a MAC Address and Why It Matters for Your Home Network | what-is-a-mac-address-and-why-it-matters-for-your-home-network | 2148 | IP & Networking (18) | https://beanel.com/what-is-a-mac-address-and-why-it-matters-for-your-home-network/ |
| howzitza.co.za | South African Street Food: A Flavourful Journey Through Mzansi | south-african-street-food-flavourful-journey-mzansi | 1733 | South African Culture (4) | https://howzitza.co.za/south-african-street-food-flavourful-journey-mzansi/ |
| sumza.co.za | How to Calculate Your Electricity Bill in South Africa: A Complete Guide | how-to-calculate-your-electricity-bill-in-south-africa | 1891 | Solar & Electricity (7) | https://sumza.co.za/how-to-calculate-your-electricity-bill-in-south-africa/ |
| zadocs.co.za | How to Write a Last Will and Testament in South Africa: A Complete Guide | how-to-write-a-last-will-and-testament-in-south-africa | 2233 | Personal (21) | https://zadocs.co.za/how-to-write-a-last-will-and-testament-in-south-africa/ |
| saymyname.co.za | Beautiful Zulu Baby Names and Their Meanings | beautiful-zulu-baby-names-and-their-meanings | 1798 | Baby Names (3) | https://saymyname.co.za/beautiful-zulu-baby-names-and-their-meanings/ |
| whippetqr.com | QR Codes for Restaurant Menus: A Complete Guide for South African Restaurants | qr-codes-for-restaurant-menus-south-african-guide | 1978 | QR Code Guides (3) | https://whippetqr.com/2026/09/07/qr-codes-for-restaurant-menus-south-african-guide/ |
| 5minutes.co.za | Classic Card Games to Play at a South African Braai | classic-card-games-to-play-at-a-south-african-braai | 2135 | South African Games (5) | https://5minutes.co.za/classic-card-games-to-play-at-a-south-african-braai/ |

## Images
- All images sourced from Wikimedia Commons (real CC-licensed photos, preferred source).
- 2 different images per article (different subjects, not just different filenames).
- Featured image at top (hero), in-content image at ~80-92% through content (between H2 sections).
- `data-no-lazy="1"` added to both images to prevent LiteSpeed lazy-load stripping.

## Sitemap
- LiteSpeed was serving stale cached sitemaps (new articles absent).
- Regenerated static `post-sitemap.xml` on all 7 sites via PHP `file_put_contents()` (bypasses Rank Math + LiteSpeed cache).
- Verified all 7 new article slugs present in regenerated sitemaps.

## Verification
- All 7 article URLs return HTTP 200 publicly (curl -sL).
- Rendered pages show 2 `<figure>` elements each, correctly positioned (Fig0 at 0%, Fig1 at 80-92%).
- All posts published with real categories (no Uncategorized).

## Cleanup
- All PHP scripts (article creation + sitemap gen) deleted from all servers after execution.
- Local scripts retained in /home/m/weekly_articles/scripts/ for reference.

## Notes
- Fixed bug in `wikimedia-commons-batch-search.py` (missing `import requests` at module level).
- All 7 sites were up (homepage 200) at start of run.
