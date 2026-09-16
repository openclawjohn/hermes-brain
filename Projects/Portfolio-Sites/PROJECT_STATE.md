# Portfolio Sites — Project State

## OVERLORD ROLLOUT — zadocs 62/62 articles fixed, independently verified (2026-09-16)

**OVERLORD layer built first (D-005):** `~/.hermes/profiles/` did not exist; no `@Developer`/`@QC-Auditor`/`@UI-UX-Designer` on any date; `kanban.orchestrator_profile` + `default_assignee` empty; `kanban.db` 0 tasks; `delegation.max_iterations` = **15** (the real cause of the recorded "14/14 subagent failure rate"). Fixed: 3 profiles on **different model families** (`developer`=deepseek-v4-pro, `qc-auditor`=kimi-k2.6, `ux-designer`=glm-5.3), orchestrator/assignee set, iterations 15→60, timeout 600→1800, routing proven with a live `ROUTING_PROOF_OK` round trip.

**zadocs result:** 62/62 articles now render **2 distinct images** (pilot 5/5 + rollout 57/57), 0 empty srcs, ≥1,500 words. Verified independently by the OVERLORD *and* by `qc-auditor` on a different model.

**Failure handled as an input:** batch 4 exhausted its iteration budget (90/90); the board auto-re-queued it; the OVERLORD re-inspected, found image 1 present / image 2 missing, and re-briefed a narrower card — which completed. No capability claim made.

**🚨 I was wrong THREE times measuring 5minutes:** `entry-content` regex → 26 bad; `<h1>`→footer → 11; minus "homepage baseline" → 16. All wrong. **A homepage screenshot is NOT site chrome — the homepage lists recent articles' featured images, so excluding them discards real article images.** Correct method: count `<img>` inside the `<article>` element, exclude only the logo. **True result: 5minutes 36/36 PASS, 0 defects.** When three attempts give three answers, the method is the bug.

**Cleanup:** `zd-embed.php`, `zd-fix.php`, `zd-diag.php`, `hm_audit.php`, `archive_diag.php` all 404.

**Still open:** zadocs hero-image duplication (3 images reused as hero across many articles — aesthetic, not a gate failure); beanel post 900 (1 image); howzitza review not submitted.

**See `2026-09-16-overlord-layer-and-zadocs-62of62.md`.**

## Updated: 2026-09-16

## Recent Work (2026-09-16)
- **Guardian run — 33 articles had <2 rendered images, all fixed (COMPLETE).** Root cause: the 5minutes/beanel themes do NOT call `the_post_thumbnail()`, so `featured_media` is stored but never rendered — posts looked image-less to visitors while the DB said otherwise. Fixed via PHP-over-FTP: hero figure embedded in `post_content` + a second different image at ~55% depth. **5minutes 26 posts** (21 via `wp_update_post`, 5 Elementor posts 132/134/136/138/140 via `_elementor_data` text-editor mutation + `files_manager->clear_cache()` — Elementor served stale output until the cache clear — plus post 330). **beanel 7 posts** (160/157/155/151/149/147/144) had the SAME image twice; 2nd figure replaced. **Verified: 0 of 36 5minutes posts render <2 content images (was 26); beanel posts show 2 distinct images each.** All deployment scripts deleted, confirmed 404. See `2026-09-16-guardian-33-articles-image-fix.md`.
- **whippetqr post 197 Orchid-ad defect from the 2026-09-15 cycle: RESOLVED.** Old ad file returns 404, current image (`2026/09/wq197-body.jpg`) vision-inspected clean.
- **beanel.com map: verified working — all 5 guardian flags are false positives.** Live logged-out check: Leaflet container ✅, zoom ✅, attribution ✅, 8/8 tiles ✅, IP `102.132.217.18` ✅, city "Mossel Bay", map 858×350. Vision confirms a real street map. The guardian reads curl source where JS-rendered markers never appear.
- **Server: all 7 sites HTTP 200, no crash/throttle.** Post counts unchanged: beanel 40, howzitza 34, sumza 39, zadocs 73, saymyname 34, whippetqr 44, 5minutes 36.
- **KEY LEARNING:** the correct image metric is "rendered `<img>` tags inside `entry-content`" — counting `featured_media` in the DB (or raw `<img>` anywhere on the page) gives false passes. Theme logos (`logo-group-photo.png`, `sumza-logo.png`, `cropped-Second-Logo.png`) live in the header and must be excluded.

## Recent Work (2026-08-20)
- **Independent AdSense Auditor built + first run (COMPLETE):** Built a standalone from-scratch auditor at `~/.hermes/scripts/adsense-auditor.py` (backed up to `github.com/openclawjohn/hermes-backup` branch `feature/adsense-auditor`). It re-checks every AdSense gate item independently against the live site — does NOT trust prior "clean" status. Uses the corrected stripped-text full-section hash for template-shell detection. Ran across all 7 sites in 154s. **Result: NONE of the 7 sites is fully AdSense-ready.** The 2026-08-08 "ALL SITES CLEAN" claim was WRONG — the auditor surfaced byte-identical template shells on 5minutes (16 posts), sumza, whippetqr, and howzitza that the prior audit missed. See `2026-08-20-independent-adsense-auditor.md`.
- **KEY FINDINGS (verified, not false positives):**
  - **5minutes:** 3 template shells — "The Educational Value of Quick Games" ×16 (436w byte-identical, 1 hash), "Why These Quick Games Matter" ×16, "Making the Most of Your Game Time" ×6
  - **sumza:** 4 template shells ("Common Misconceptions" ×13, "Practical Next Steps" ×5, "Summary" ×5, "UIF Claims Process" ×2) + 3 duplicate H2s
  - **whippetqr:** 13 duplicate H2s + 2 template shells ("FAQ About QR Codes" ×6 = 971w identical, "Practical Tips" ×3)
  - **howzitza:** only 18 posts (need 30+), 1 dup H2, 1 template shell, 1 Uncategorized post, 3 image-less posts
  - **saymyname:** only 29 posts (need 30+), 4 posts under 1,500 words, 1 image-less post
  - **beanel:** 2 posts <2 images; **zadocs:** 3 posts <2 images + 7 posts boilerplate markers
- **KEY LEARNING:** The 2026-08-08 "ALL SITES CLEAN" was based on the flawed raw-HTML hash methodology. The independent auditor (stripped-text full-section hash) is now the source of truth. Re-run it after any fix to confirm 0 FAILs.

## Recent Work (2026-08-19)
- **SayMyName low-value-content fix (COMPLETE):** Google flagged saymyname for "Low value content." Root cause was **byte-identical boilerplate sections** reused across 20 of 29 articles ("The Significance/Importance of Names in South African Culture", "More About African Naming Traditions", "Choosing a Business Name") — the correct stripped-text hash test proved them factory copies. Authored unique 1,500+ word top-ups for all 20, removed all boilerplate, deployed via direct `$wpdb->update`, purged LiteSpeed. **Verified: 0 boilerplate sections remain, all 20 posts ≥1,500 words.** Earlier "Clean" status was wrong (hashed raw HTML, not stripped text). See `2026-08-19-saymyname-low-value-content-fix.md`.
- **SayMyName image pass (COMPLETE):** All 23 saymyname articles that lacked 2 images now have **2 distinct, on-topic, real CC-licensed Wikimedia photos** each (hero top + in-content ~55%). Sourced via Wikimedia Commons API + mandatory `vision_analyze` pixel inspection (rejected elephants, "AMERICAN CULTURE" sign, numbered headbands, boaters/QR watermarks). Uploaded via FTP, deployed featured + embedded content `<figure>` via PHP (theme ignores `featured_media`, so hero must be in-content), deduped double-embeds, purged LiteSpeed. **Verified: 2 distinct images render on all 29 articles (curl, 0 issues).** Temp PHP scripts deleted.
- **KEY LEARNING:** The template-shell detection MUST hash **stripped plain text** per section, NOT raw HTML. Raw HTML differs (image tags/whitespace) even when the text is a copy-paste, which caused saymyname to be falsely marked "Clean" on 2026-08-08/18.

## All 7 Sites — Content Status

> **⚠️ 2026-08-20: "ALL SITES CLEAN" (2026-08-08) was WRONG.** The independent auditor (stripped-text full-section hash) found byte-identical template shells on 5minutes, sumza, whippetqr, and howzitza that the prior audit missed. **None of the 7 sites is fully AdSense-ready.** See `2026-08-20-independent-adsense-auditor.md` for the full findings and fix list.

| Site | Posts | @1,500+ | Images | Auditor status (2026-08-20) |
|------|:-----:|:-------:|:------:|:-------|
| **sumza.co.za** | 35 | ✅ | ✅ | ❌ 4 template shells + 3 dup H2s |
| **howzitza.co.za** | 18 | ✅ | 3 posts <2 | ❌ only 18 posts (need 30+), 1 dup H2, 1 template shell, 1 Uncategorized |
| **saymyname.co.za** | 29 | **4 posts <1,500** | 1 post <2 | ❌ only 29 posts (need 30+), 4 short posts |
| **5minutes.co.za** | 32 | ✅ | 3 posts <2 | ❌ 3 template shells (16+16+6 posts) |
| **whippetqr.com** | 40 | ✅ | 3 posts <2 | ❌ 13 dup H2s + 2 template shells |
| **zadocs.co.za** | 69 | ✅ | 3 posts <2 | ❌ 3 posts <2 images, 7 posts boilerplate markers |
| **beanel.com** | 36 | ✅ | 2 posts <2 | ❌ 2 posts <2 images |
| **Total** | **259** | | | **❌ 0/7 fully ready** |

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
