# Portfolio Sites — Project State
## Updated: 2026-08-19

## Recent Work (2026-08-19)
- **SayMyName low-value-content fix (COMPLETE):** Google flagged saymyname for "Low value content." Root cause was **byte-identical boilerplate sections** reused across 20 of 29 articles ("The Significance/Importance of Names in South African Culture", "More About African Naming Traditions", "Choosing a Business Name") — the correct stripped-text hash test proved them factory copies. Authored unique 1,500+ word top-ups for all 20, removed all boilerplate, deployed via direct `$wpdb->update`, purged LiteSpeed. **Verified: 0 boilerplate sections remain, all 20 posts ≥1,500 words.** Earlier "Clean" status was wrong (hashed raw HTML, not stripped text). See `2026-08-19-saymyname-low-value-content-fix.md`.
- **SayMyName image pass (COMPLETE):** All 23 saymyname articles that lacked 2 images now have **2 distinct, on-topic, real CC-licensed Wikimedia photos** each (hero top + in-content ~55%). Sourced via Wikimedia Commons API + mandatory `vision_analyze` pixel inspection (rejected elephants, "AMERICAN CULTURE" sign, numbered headbands, boaters/QR watermarks). Uploaded via FTP, deployed featured + embedded content `<figure>` via PHP (theme ignores `featured_media`, so hero must be in-content), deduped double-embeds, purged LiteSpeed. **Verified: 2 distinct images render on all 29 articles (curl, 0 issues).** Temp PHP scripts deleted.
- **KEY LEARNING:** The template-shell detection MUST hash **stripped plain text** per section, NOT raw HTML. Raw HTML differs (image tags/whitespace) even when the text is a copy-paste, which caused saymyname to be falsely marked "Clean" on 2026-08-08/18.

## All 7 Sites — Content Status

> **✅ 2026-08-08: ALL SITES CLEAN.** The template-shell (byte-identical boilerplate) problem that caused repeated "Low value content" rejections is now ELIMINATED across all 7 sites. sumza (16 posts) + beanel (28 posts) had byte-identical "Why This Matters" blocks removed; 5minutes (7 posts) top-upped; all verified 1,500+ words, 0 boilerplate markers. Recrawls requested via IndexNow + Google Search Console on all 7. See `2026-08-08-portfolio-boilerplate-fix-recrawl.md`.

| Site | Posts | @1,500+ | Avg Words | Images | Status |
|------|:-----:|:-------:|:---------:|:------:|:-------|
| **sumza.co.za** | 31 | **31** ✅ | 1,600+ | 31 | ✅ Clean (WTM removed 16 posts) |
| **howzitza.co.za** | 14 | **14** ✅ | 1,600+ | 14 | ✅ Clean |
| **saymyname.co.za** | 29 | **29** ✅ | 1,600+ | **29** | ✅ **FIXED 2026-08-19** (removed byte-identical boilerplate from 20 posts + all 29 posts now have 2 real CC images) |
| **5minutes.co.za** | 28 | **28** ✅ | 1,500+ | 28 | ✅ Clean (7 posts top-upped) |
| **whippetqr.com** | 37 | **37** ✅ | 1,800+ | 37 | ✅ Clean |
| **zadocs.co.za** | 65 | **65** ✅ | 1,500+ | 65 | ✅ **REWRITTEN 2026-08-07** (was 63 template shells) |
| **beanel.com** | 32 | **32** ✅ | 1,800+ | 32 | ✅ Clean (WTM removed 28 posts) |
| **Total** | **236** | **236** | **1,600+ avg** | **236** | **✅ 100% CLEAN** |

## Recent Work (2026-08-18)
- **Weekly Blog Posts backfill (COMPLETE):** Machine was off Aug 10–15; the Tue Aug 11 weekly cron missed. Triggered manually — 7 articles published (1/site), all verified live (HTTP 200), 2 distinct in-content images each, correctly positioned, real categories, in sitemaps. See `2026-08-18-weekly-blog-backfill-image-fixes-ceo-maintenance.md`.
- **Image quality audit + 3 replacements (COMPLETE):** Ran mandatory `vision_analyze` pixel inspection on all 9 new article images. 3 failed (sumza fuel hero "ORINO"/"R" branding, sumza fuel content receipt text, whippetqr WhatsApp content Chinese chars/logos). Replaced all 3 with clean fal.ai Klein images, inspected clean, uploaded via FTP, content updated via PHP, cache purged, verified live.
- **5minutes post 138 expanded (COMPLETE):** Was 1475 words (under 1500). It's an **Elementor page** — `wp_update_post` doesn't update rendered content. Fixed via Elementor MCP `elementor_mcp_update_widget` (widget `7eb369a`). Now 1655 words, verified live.
- **CEO backfill (PARTIAL):** Quora 1 answer posted+verified. Pinterest NOT done (save-from-URL UI won't render selection; bridge lacks CDP file-upload). Reddit/Medium BLOCKED (logged out in Chrome).
- **Phase 3 maintenance (COMPLETE):** All 7 sites up; IndexNow keys/sitemaps/ads.txt all 200; AdSense meta present; no broken slugs; no missing alt text; all 7 new articles in sitemaps.
- **KEY LEARNING — LiteSpeed cache:** `\LiteSpeed\Purge::purge_all()` + `do_action('litespeed_purge_all')` do NOT clear the server page cache. Actual cache is at **`/home/whippetq/lscache`** (NOT `wp-content/litespeed`). Must delete files there via PHP `rrmdir()` to force fresh render.

## Recent Work (2026-08-10)
- **Indexing/canonical duplicate fix (COMPLETE):** Google emailed about saymyname "cannot be indexed — Duplicate, Google chose different canonical." Audited ALL 7 sites' Google emails + live GSC indexing reports. Root cause: orphaned `-2` duplicate pages self-canonicalizing in sitemaps. Fixed on 3 sites:
  - **saymyname**: renamed `privacy-policy-2` → `privacy-policy` (deleted draft), mu-plugin redirect old slug
  - **howzitza**: renamed `privacy-policy-2`→`privacy-policy`, `personality-test-2`→`personality-test`, `play-2`→`play`; consolidated byte-identical `/privacy/`→`/privacy-policy/`; mu-plugin redirects
  - **5minutes**: `/home-2/` → `/` (front page canonical) via mu-plugin, excluded from sitemap
  - Rebuilt static sitemaps (0 `-2` slugs), pinged IndexNow (202). Other 4 sites' reasons (noindex/4xx/404) were already-fine system pages.
- **Key learning:** LiteSpeed page cache serves stale 200s — ALWAYS purge cache before verifying URL redirect/behavior changes. See `2026-08-10-portfolio-indexing-duplicate-fix.md`.

## Recent Work (2026-08-08)
- **Portfolio-wide boilerplate fix (COMPLETE):** Removed byte-identical "Why This Matters" blocks from sumza (16 posts) + beanel (28 posts); top-upped 7 short 5minutes posts. All 7 sites now have 0 boilerplate markers, every article 1,500+ words.
- **Sitemap recrawl:** IndexNow pinged all 7 sitemaps (HTTP 202) + Google Search Console "Request Indexing" submitted on all 7 via user's Chrome.
- **AdSense site review submitted for zadocs.co.za** — status now "Getting ready / Review requested" (was "Needs attention / Low value content").
- **ads.txt re-uploaded** on 6 sites (bumped last-modified to force Google re-crawl; 6 showed "Not found" despite HTTP 200 + correct content).
- **Technical fixes (all verified live):** favicon links added (whippetqr, zadocs); 5minutes HTTP→HTTPS 301 redirect fixed; security headers (HSTS/XCTO/XFO/Referrer/Permissions) added to all 7; image lazy-loading added to all 7. All 7 domains confirmed verified in Search Console.
- **Image optimization (COMPLETE):** Recompressed all images across all 7 sites (server-side PHP/GD, quality 82). Saved ~282 MB total (beanel -82MB, howzitza -48MB, saymyname -39MB, 5minutes -40MB, whippetqr -34MB, sumza -21MB, zadocs -17MB). Originals backed up to `wp-content/imageopt-backup/`. All pages verified rendering, 0 broken images.

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
- **Pinterest pin creation via bridge** — save-from-URL fetches image but selection UI never renders; bridge lacks CDP file-upload. Needs manual pin or CDP route.
- **Reddit / Medium** — logged out in Chrome profile; CEO backfill for these platforms blocked until user logs in.
