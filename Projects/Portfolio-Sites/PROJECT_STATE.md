# Portfolio Sites — Project State
## Updated: 2026-08-03

## All 7 Sites — Content Status

> **⚠️ 2026-08-07 CRITICAL:** The "1,500+ words" figures below masked a hidden AdSense killer: **byte-identical template shells.** zadocs had 63/66 posts sharing the same factory boilerplate (hash-identical "Understanding South African Law" tail), which is why it kept getting "Low value content" rejections despite passing word-count box-checks. See `2026-08-07-zadocs-adsense-content-rewrite.md`. zadocs is now genuinely rewritten; **the other 5 sites below still have template-signature content and need the same treatment.**

| Site | Posts | @1,500+ | Avg Words | Images | Status |
|------|:-----:|:-------:|:---------:|:------:|:-------|
| **sumza.co.za** | 31 | **31** ✅ | 1,600+ | 31 | ⚠️ "Why This Matters" ×16, FAQs ×21 |
| **howzitza.co.za** | 14 | **14** ✅ | 1,600+ | 14 | ✅ Mostly clean |
| **saymyname.co.za** | 29 | **29** ✅ | 1,600+ | 29 | ⚠️ "Significance of Names" ×22 |
| **5minutes.co.za** | 28 | **28** ✅ | 1,500+ | 28 | ⚠️ "Educational Value of Quick Games" ×16 |
| **whippetqr.com** | 37 | **37** ✅ | 1,800+ | 37 | ⚠️ "Conclusion" ×47, FAQs ×23 |
| **zadocs.co.za** | 65 | **65** ✅ | 1,500+ | 65 | ✅ **REWRITTEN 2026-08-07** (was 63 template shells) |
| **beanel.com** | 32 | **32** ✅ | 1,800+ | 32 | ⚠️ "Why This Matters" ×28 |
| **Total** | **236** | **236** | **1,600+ avg** | **236** | **zadocs fixed; 5 others need rewrite** |

## Recent Work (2026-08-07)
- **ZADocs content rewrite (COMPLETE):** 63 template pages rewritten with unique 1,500+ word content. Zero boilerplate headers remain. Published via direct-SQL (wp_update_post hangs on cp47). Verified: all 66 posts clean, 2+ images, sitemap matches.
- **Skills:** adsense-quality-debug (Trigger 8 template shells), portfolio-site-quality-standards, zadocs-maintenance (cp47 publish method).
- **Branch:** `fix/adsense-content-rewrite` on zadocs-wordpress.

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
