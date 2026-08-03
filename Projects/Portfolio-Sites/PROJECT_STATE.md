# Portfolio Sites — Project State
## Updated: 2026-08-03

## All 7 Sites — Content Status

| Site | Posts | @1,500+ | Avg Words | Images | Status |
|------|:-----:|:-------:|:---------:|:------:|:-------|
| **sumza.co.za** | 31 | **31** ✅ | 1,600+ | 31 | ✅ Complete |
| **howzitza.co.za** | 14 | **14** ✅ | 1,600+ | 14 | ✅ Complete |
| **saymyname.co.za** | 29 | **29** ✅ | 1,600+ | 29 | ✅ Complete |
| **5minutes.co.za** | 28 | **28** ✅ | 1,500+ | 28 | ✅ Complete |
| **whippetqr.com** | 37 | **37** ✅ | 1,800+ | 37 | ✅ Complete |
| **zadocs.co.za** | 65 | **65** ✅ | 1,500+ | 65 | ✅ Complete |
| **beanel.com** | 32 | **32** ✅ | 1,800+ | 32 | ✅ Complete |
| **Total** | **236** | **236** | **1,600+ avg** | **236** | **✅ 100%** |

## Recent Work (2026-08-03)
- **Article expansions:** 12 articles expanded to 1,500+ words across whippetqr (3), howzitza (3), saymyname (3), 5minutes (3)
- **Redirect fixes:** whippetqr /contact/ and zadocs /contact/ now 200 (were 301)
- **Sitemaps:** Regenerated on zadocs, saymyname, 5minutes (lastmod updated Jul 24 → Aug 3)
- **IndexNow:** All 7 sites pinged after content changes
- **CEO skill:** Updated to v2.1 with article quality, sitemap freshness, and auto-ping checks

## CEO of Domains — Daily Cron
- Runs daily at 08:00 (job eb66b3bea877)
- Weekly rotation: Mon=cross-links, Tue=Reddit, Wed=Quora, Thu=Pinterest, Fri=LinkedIn/Quora, Sat=SEO, Sun=research
- v2.1 skill includes: fix-everything mandate, article quality checks, sitemap freshness, IndexNow auto-ping

## Credentials — Single Source of Truth
- `/home/m/credentials.json`
- `/home/m/SITE_CREDENTIALS.md`

## Known Issues
- **Beanel FTP access** — broken, cannot fix contact redirect or sitemap remotely
- **Sitemap regeneration** — failed on whippetqr, howzitza, sumza, beanel (PHP error on those sites)
- **REST API** — application passwords lack edit permissions, must use PHP/FTP for content updates
