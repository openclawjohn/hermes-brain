# Portfolio Sites — Project State
## Updated: 2026-09-15 (evening)

## AdSense Feedback Read Live + Quality Fixes (2026-09-15 ~18:50 SAST)
- **AdSense read directly via Chrome Connector bridge — no guessing.** Real verdict: **only ONE site has a policy violation — howzitza.co.za = "Needs attention / Low value content", flagged Sep 12, 2026 6:10 AM SAST.** The other six are "Getting ready" with no violation (beanel, sumza, zadocs, saymyname, 5minutes, whippetqr).
- **ads.txt "Not found" on 5 sites is NOT a file problem.** All 7 serve identical 59-byte files with correct pub ID + trailing newline, all 200, all serving 200 to AdsBot-Google/Googlebot/Mediapartners-Google. howzitza + whippetqr show Authorized with byte-identical files → the difference is Google-side crawl recognition. **Do NOT re-upload with a new timestamp — tried across multiple sessions, not a new fix.**
- **Search Console: all 7 domains verified properties; every sitemap Success** (read Sep 10–14). Note sumza + whippetqr each have TWO sitemaps submitted (native `wp-sitemap.xml` + Rank Math `sitemap_index.xml`) — untidy, both Success.
- **FIXED — broken empty-src images** (`<figure>` wrapping `<img src="">`, renders a broken image icon), all verified on the rendered page: howzitza 553 (×2), sumza 747 + 742 (×1 each), zadocs 1178 (×2), **beanel 1000 (×2)**, **whippetqr 591 (×2)**.
- **FIXED — howzitza duplicate/near-duplicate titles.** Renamed 457, 327, 301, 413. Now **0 duplicate titles**.
- **FIXED — byte-identical replicated content.** 5minutes: 3 posts shared a 442-word block (hash 7b2fadf46c84) → replaced with unique prose. whippetqr: 3 posts shared an identical Conclusion (71w) + 3 posts shared an identical Conclusion (74w) AND a 781-word FAQ block → all 9 blocks replaced. **Re-check: 0 byte-identical sections across 5minutes/howzitza/zadocs/saymyname/whippetqr.**
- **FULL CORRECTED PORTFOLIO IMAGE SCAN — 300 articles** (`~/.hermes/scripts/portfolio-image-scan-corrected.py` → `/home/m/portfolio-image-scan-corrected.json`). Final: **beanel 0, howzitza 0, sumza 0, saymyname 0, whippetqr 0, zadocs 62, 5minutes 26** with real defects. See the two sections below.
- **Housekeeping:** all fixer PHP deleted + 404-verified; sidecar dirs removed; **33 publicly-reachable content-mutating helper scripts quarantined** to `_hermes_quarantine/` (18 on cp47: zadocs archive_diag/create_articles_page/hm_audit/zd_diag/zd_diag2/zd_purge + other sites' archive_diag + public_html; **15 on beanel**: weekly_article.php, weekly_aug16.php, beanel_article.php, beanel_purge.php, c.php, d.php, h.php, i.php, k.php, s.php, cleanup.php, fix-fn.php, fix-uncategorized-beanel.php, fix_alt_simple.php, fix_titles_beanel.php). Only WP core PHP now reachable on beanel. Chrome tab closed.
- **See `2026-09-15-adsense-feedback-and-quality-fixes.md`.**

## 🚨 MY FIRST IMAGE SCANNER WAS BROKEN — use the CORRECTED extractor
The first scan (`portfolio-image-scan.py`, `/home/m/portfolio-image-scan.json`) **over-reported by a factor of 10**. It flagged **125** posts portfolio-wide including **whippetqr 32** and **5minutes 26**. Spot-checking 6 whippetqr "broken" posts with a correct extractor showed **2 images and 1,718–2,308 words each — not broken at all.**
- **Root cause:** the script isolated the article body with a `class="...entry-content..."` regex. On these themes `entry-content` also appears **inside an inline `<style>` block** (e.g. `.page .entry-content { margin: 0 !important; }`), so the regex matched a CSS rule, the body was truncated to a fragment, and the post was reported as `imgs=0`.
- **The fix:** strip `<script>`/`<style>`, then take from the first `<h1>` to `<footer>`/`</article>`. Also exclude `custom-logo` and count empty-src `<img>` explicitly.
- **TRUST THE CORRECTED FILE:** `/home/m/portfolio-image-scan-corrected.json` (300 articles, 91 with issues). Delete/ignore the old JSON and script.
- **Lesson: a scanner that reports a large number is not evidence. Spot-check several "failing" items with an independent method before acting on a bulk result** — this false alarm almost triggered rewriting 32 articles that were never broken.

## 🚨 CORRECTION to the 06:17 cycle claim below — zadocs and 5minutes DO NOT render article images
The 06:17 note says broken articles were only missing *in-content* images and that "Themes render the featured image fine — verified in live HTML on all 4 affected sites." **That is wrong.** Confirmed on rendered pages 2026-09-15 evening:

| Site | Post | Visible `<img>` in article body | Where the featured image actually is |
|---|---|---|---|
| zadocs | 16 `leave-application-form` | **2 — both the theme logo** (`cropped-Second-Logo.png`) | only in `og:image` / `twitter:image` / schema `primaryImageOfPage` |
| zadocs | `lease-agreement-template`, `employment-contract-template` | same (2 logo imgs) | same |
| 5minutes | 132 `find-the-springbok…` | **2 — both the site logo** | only in the Elementor JSON config + `og:image` |

Zero `wp-post-image`, zero `<picture>`, zero background-image refs, zero `<img>` for the featured image on those pages. Astra declares `has-post-thumbnail` but renders nothing. **So `_thumbnail_id`/`featured_media` being set does NOT mean the image renders.**

- **zadocs: 62 of 73 articles show a visitor no article image at all.**
- **5minutes: 26 of 36.** The **10 newest render 2 images each** (e.g. 592 baby-shower → `…-1.jpg` + `…-2.jpg`; 585 wedding → `wedding-games-table.jpg` + `wedding-games-guests.jpg`) — **which proves the theme CAN render them; the older posts simply never had the embed inserted.**
- **Secondary:** duplicate featured images — zadocs' 73 articles use only **13 distinct** featured images (media 760 ×21, 759 ×20, 758 ×20). Even if the theme rendered them, 61 articles would show near-identical pictures. Needs genuinely new topical images — a content-generation pass.
- **Next job.** 88 articles (62 + 26) need the image embed inserted, and 61 zadocs featured images need replacing with distinct topical photos.

## Monitor Cycle (2026-09-15 06:17 SAST) — Phase F stable + AdSense blocker quantified (CORRECTION)
- **Status:** all 7 homepages 200 across 3 passes, zero oscillation, sitemap↔REST parity 1:1, essentials 200, ads.txt 200 ×7, AdSense meta 1 ×7, 0 real broken slugs. Counts identical to 06:11.
- **🚨 CORRECTION — the previous cycle's image alarm was a false alarm.** "87 zero-image / 45 one-image Articles" and "13 Articles with no featured image" were **external curl throttle artifacts**, not site state. Proof: zadocs `/how-to-draft-a-rental-agreement-in-south-africa/` was counted as 0 images but renders 2. A burst request returns **partial HTML with HTTP 200 and full `size_download`** — silent truncation. **Never derive an image count from externally-fetched `content.rendered` on this server.**
- **AUTHORITATIVE image inventory (server-side PHP):** 115 Articles have **no in-content image** — zadocs 61, whippetqr 29, 5minutes 21, howzitza 4. **Zero** Articles are missing a featured image portfolio-wide (`_thumbnail_id` set on all 115).
- **Root cause:** every broken Article was written by a pass that never inserted an in-content image (`post_content` = `figure=0 img=0 wp:image=0`). Themes render the featured image fine — verified in live HTML on all 4 affected sites.
- **Secondary:** duplicate featured images — zadocs serves 73 Articles from only **13 distinct** featured images. Same shared-skeleton defect behind the AdSense "low value content" history.
- **Not fixed (deliberate):** filename-based auto-assignment was evaluated and **rejected on evidence** (one baby-shower image matched 5 unrelated 5minutes Articles; 1/61 strong matches on zadocs). Needs new distinct topical images — a content-generation pass, not a copy pass.
- **Cleaned:** 4 diagnostic PHP scripts removed from public web roots, verified 404.
- See `2026-09-15-0617-portfolio-monitor-cycle.md`.

## Monitor Cycle (2026-09-15 04:35 SAST) — Phase F stable + 2 REPAIRS + attack-surface cleanup
- **Status:** all 7 homepages 200 across 3 passes, zero oscillation, sitemap↔REST parity 1:1, essential pages OK, ads.txt 200 ×7, AdSense meta 1 ×7, 0 real broken slugs. No status change vs 04:21.
- **REPAIR 1 — sumza.co.za sitemaps:** deleted by an accidental execution of the leftover `del_sitemap_sumza.php` helper during a `curl -sI` probe. Regenerated static `post-sitemap.xml` (39 urls), `page-sitemap.xml` (61 urls), `sitemap_index.xml`; all 200 now.
- **REPAIR 2 — privacy-policy redirect loop** on howzitza.co.za + saymyname.co.za: `/privacy-policy/` ↔ `/privacy-policy-2/` infinite 301, page unreachable. Page carried slug `privacy-policy-2` while no page owned `privacy-policy`. Renamed both to canonical `privacy-policy`; both 200, no redirect.
- **CLEANED — 208 publicly-reachable content-mutating helper PHP scripts** quarantined by FTP rename into `_hermes_quarantine/` (recoverable, not deleted) across the 6 cp47 web roots; 11 own `hermes-*.php` helpers deleted. Only WP core remains reachable.
- **CORRECTED:** sumza robots.txt declares `wp-sitemap.xml` (native) — its `sitemap_index.xml` 404 is not a fault.
- **🚨 INCIDENT:** `curl -sI` sends HEAD and PHP still executes on HEAD — probing leftover helpers ran them. No content lost (verified: counts + `post_modified` timestamps unchanged). Lesson added to the audit skill: never request an unknown PHP file on a live site, read it over FTP first.
- See `2026-09-15-0435-portfolio-monitor-cycle.md`.

## Recent Work (2026-08-24) — FIX-EVERYTHING WARN CLEANUP
- **Cache-Control headers added to all 7 sites** (`.htaccess`) — fixes `caching_headers` WARN.
- **Author bylines added to beanel + whippetqr** (E-E-A-T mu-plugin) — fixes `author_byline`.
- **Sitemap lastmod regenerated on 4 sites** (beanel, zadocs, whippetqr, 5minutes) — fixes `sitemap_lastmod` + `modified_dates_consistent`.
- **A11y + perf mu-plugin on all 7** (font-size floor, tap-targets, script deferral) — fixes `font_size`, `tap_targets`, `render_blocking`.
- **WebP conversion + picture-wrap on all 7** (on-server GD + mu-plugin) — fixes `image_formats`.
- **18 over-long titles trimmed** (sumza, zadocs, saymyname, 5minutes) — fixes `title_descriptive`.
- **Auditor false-positives fixed** (HSTS lowercase bug, better_ads_standards animation) — removes `security_headers` + `better_ads_standards` noise.
- See `2026-08-24-fix-everything-warn-cleanup.md`.

## Auditor Courses 7-10 (2026-08-22, prior)
- Auditor grew 60 → 81 checks (Performance, A11y/UX, Security/Tech-SEO, Ad-Implementation).
- CEO fixes: sumza canonical-conflict FAIL, cookie-consent banners on 4 sites, duplicate ad scripts, security headers, TTFB.
- All 7 sites passed 0 FAILs. See `2026-08-22-auditor-courses-7-10-evaluation.md`.

## All 7 Sites — Content Status (2026-08-24)

> **✅ ALL 7 SITES PASS the independent AdSense auditor with 0 FAILs** (81 checks). WARN count reduced from 56 by the 2026-08-24 fix-everything pass. The auditor checks 80+ gates: post count, word floor, no dup H2s, no template shells, no Uncategorized, sitemap↔REST match, no broken URLs, no `-2` slugs, essential pages 200, ads.txt, AdSense meta, noindex, GA4, canonical, OG, favicon, SSL redirect, 2+ images, privacy policy, contact page, Articles nav, ad density, replication, privacy disclosure, language, viewport, title/meta, H1, image alt, internal links, canonical-self, sitemap https, duplicate titles, schema, author byline, image dims, heading hierarchy, sitemap lastmod, caching headers, image formats, TTFB, render blocking, aria labels, font size, tap targets, heading order, ad implementation, cookie consent, TLS, mixed content, security headers.

| Site | FAILs | Ready? |
|------|:-----:|:------:|
| **sumza.co.za** | 0 | ✅ |
| **howzitza.co.za** | 0 | ✅ |
| **saymyname.co.za** | 0 | ✅ |
| **5minutes.co.za** | 0 | ✅ |
| **whippetqr.com** | 0 | ✅ |
| **zadocs.co.za** | 0 | ✅ |
| **beanel.com** | 0 | ✅ |
| **Total** | **0** | **7/7 ✅** |
  - **Course 7 (Performance):** `caching_headers`, `image_formats`, `ttfb`, `render_blocking`, `duplicate_ad_scripts` (5 new).
  - **Course 8 (Accessibility/UX):** `aria_labels`, `font_size`, `heading_order`, `link_text`, `tap_targets` (5 new).
  - **Course 9 (Security/Tech SEO):** `tls_version`, `mixed_content`, `redirect_chain`, `security_headers_full`, `cookie_consent`, `canonical_conflict` (6 new).
  - **Course 10 (Ad Implementation):** `ad_units_present`, `ad_slot_valid`, `ad_unit_sizes`, `ad_to_content_mobile`, `ad_refresh` (5 new).
- **CEO fixes (done directly via FTP/PHP):**
  - Course 7: removed `duplicate_ad_scripts` on beanel/howzitza/sumza/saymyname/5minutes; fixed security_headers (beanel), ttfb (whippetqr).
  - Course 8: fixed security_headers on zadocs.
  - Course 9: **sumza canonical_conflict FAIL fixed** (3 duplicate canonicals from theme SEO functions removed); **cookie_consent banners deployed** to howzitza/sumza/saymyname/5minutes (portfolio-cookie-consent mu-plugin); security_headers fixed on sumza/5minutes; ttfb fixed on whippetqr.
  - Course 10: all 5 new ad checks PASS on all 7 sites from the first run — ad implementation was already compliant. Only new WARN: `better_ads_standards` on 5minutes (non-blocking).
- **Fresh full audit (2026-08-22): ALL 7 SITES 0 FAILs** across all 81 checks. `adsense-audit-report-final2.json`.
- **Verdict: All 7 sites are AdSense-ready. Nothing blocking left to fix.** Remaining WARNs are non-blocking quality polish (accessibility, CWV/performance, content/SEO). See `2026-08-22-auditor-courses-7-10-evaluation.md`.
- **⚠️ Process gap found:** courses 7, 8, 10 never wrote their Obsidian docs (only course 9 + JSON reports exist). Mandatory workflow must include the Obsidian doc for EVERY course.

## Recent Work (2026-08-21) — Auditor Course 9: Security & Technical SEO
- **Course 9 (Security & Technical SEO Deep Dive) COMPLETE:** Researched Google Search Central docs (HTTPS/TLS, canonical, robots-meta, url-structure, redirects) + OWASP security headers + GDPR/consent. Added **6 new checks** to the auditor: `tls_version`, `mixed_content`, `redirect_chain`, `security_headers_full`, `cookie_consent`, `canonical_conflict`. (Note: web_search/web_extract tools were down — firecrawl key unset — so research used direct curl.)
- **Found + fixed (directly via FTP/PHP):**
  - **sumza.co.za canonical conflict (FAIL):** theme `functions.php` had 3 custom SEO functions emitting duplicate canonicals. Removed the 3 duplicate `<link rel="canonical">` echo lines (kept meta desc/OG). Now 1 canonical.
  - **Cookie consent missing on howzitza, sumza, saymyname, 5minutes (WARN):** deployed a self-contained `portfolio-cookie-consent.php` mu-plugin (Accept/Reject banner + consent cookie) to all 4 sites. Purged LiteSpeed.
- **Re-audit: ALL 7 SITES 0 FAILs.** See `2026-08-21-auditor-course9-security-seo.md`.

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

## Recent Work (2026-09-15)
- **Portfolio monitor cycle 06:43 SAST (Phase F maintenance, stable):** All 7 sites 200 across 3 passes, zero oscillation. Sitemap↔REST parity 1:1 on all 7 (40/34/39/73/34/44/36). ads.txt 200 + AdSense meta = 1 on all 7. Essential pages all 200; every variant 301 resolves to the canonical. Zero `-2`/`-3` slugs. No status change vs the 06:17 cycle.
- **howzitza.co.za image gaps CLOSED (34/34 Articles now have 2 in-body images):** Fixed posts 221, 301, 319, 327 (+2 in-body images each), 342 (+1), and assigned featured thumbnails to 413 and 424. Two distinct root causes confirmed: (1) those posts were written without an in-content image; (2) **howzitza's theme does not render the featured image at all** — post 221 had `featured_media` set yet the live logged-out page had 0 `<img>` tags, so only in-body images reach visitors. All 10 images visually vetted with `vision_analyze`; 6 candidates rejected for branding/text (Durex, Coca-Cola, SPAR, Flora) or blur. Positioning verified on the live rendered page — initial insert stacked both figures at the same h2 depth, a reposition pass fixed it. Server-side re-read: 0 posts with <2 images, 0 missing featured images, min word count 1,566. See `2026-09-15-0643-portfolio-monitor-howzitza-images.md`.
- **Portfolio monitor cycle 04:21 SAST (Phase F maintenance, stable):** All 7 sites 200 across 2 passes, zero oscillation. Sitemap↔REST parity 1:1 on all 7 (40/34/39/73/34/44/36). ads.txt 200 + correct pub ID on all 7. AdSense meta tag = 1 on all 7. Essential pages all 200. Zero `-2`/`-3` slugs. Delta vs 02:14 cycle: +1 article per site from the external weekly-articles cron (02:47 SAST) — content operation, not a recovery signal.
- **Housekeeping (completed from previous run's unfinished list):** deleted orphaned `saymyname.co.za/topup2.php` via FTP (0-byte but publicly reachable HTTP 200; now 404). Verified `topup_zadocs.php`/`topup_5minutes.php`/`howzitza topup.php` already absent. See `2026-09-15-0421-portfolio-monitor-cycle.md`.

## Recent Work (2026-08-10)
- **Indexing/canonical duplicate fix (COMPLETE):** Google emailed about saymyname "cannot be indexed — Duplicate, Google chose different canonical." Audited ALL 7 sites' Google emails + live GSC indexing reports. Root cause: orphaned `-2` duplicate pages self-canonicalizing in sitemaps. Fixed on 3 sites:
  - **saymyname**: renamed `privacy-policy-2` → `privacy-policy` (deleted draft), mu-plugin redirect old slug
  - **howzitza**: renamed `privacy-policy-2`→`privacy-policy`, `personality-test-2`→`personality-test`, `play-2`→`play`; consolidated byte-identical `/privacy/`→`/privacy-policy/`; mu-plugin redirects
  - **5minutes**: `/home-2/` → `/` (front page canonical) via mu-plugin, excluded from sitemap
  - Rebuilt static sitemaps (0 `-2` slugs), pinged IndexNow (202). Other 4 sites' reasons (noindex/4xx/404) were already-fine system pages.
- **Key learning:** LiteSpeed page cache serves stale 200s — ALWAYS purge cache before verifying URL redirect/behavior changes. See `2026-08-10-portfolio-indexing-duplicate-fix.md`.

## Recent Work (2026-09-15 08:10)
- **Monitor cycle (Phase F, no status change, all 7 UP):** Housekeeping pending from 07:53 executed — 19 stale artifacts (helper PHP, image dirs, `.txt` dumps, `.html` drafts) quarantined by FTP rename to `/_hermes_quarantine/`, each verified 404; all 7 homepages re-verified 200 after removal. Verified 0 posts referenced `imgfix_tmp` before removing those dirs.
- **Sitemap gap = 0 on every site.** Prior Rank Math exclusion gaps (beanel 31/16, sumza 27/22, saymyname 31/17, 5minutes 40/13) resolved. Content counts up across sites from the Tuesday 02:00 weekly-articles cron.
- **whippetqr sitemap corruption RESOLVED:** robots.txt now declares `sitemap_index.xml` (Rank Math); was `wp-sitemap.xml` returning homepage HTML. Long-standing "worst-positioned" defect closed.
- **Broken slugs (-2/-3) = 0 on all 7** — backlog cleared.
- **Remaining content work:** 82 Articles with no in-body image (zadocs 61, 5minutes 21). 0 Articles missing featured image portfolio-wide. zadocs serves 73 Articles from only 13 distinct featured images (shared-skeleton defect behind AdSense rejections).
- **Total Articles: 300** (beanel 40, howzitza 34, sumza 39, zadocs 73, saymyname 34, whippetqr 44, 5minutes 36).
- Doc: `2026-09-15-0810-portfolio-monitor-housekeeping-and-gap-verify.md`

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
- ~~**Sitemap regeneration** — failed on whippetqr, howzitza, sumza, beanel~~ **RESOLVED (verified 2026-09-15 08:10):** all sites serve valid XML with loc counts matching REST `x-wp-total`. whippetqr now uses `sitemap_index.xml` (Rank Math).
- **REST API** — application passwords lack edit permissions, must use PHP/FTP for content updates
- **82 Articles have no in-body image** (zadocs 61, 5minutes 21) — needs a content-generation pass with new subject-checked images, not reuse
- **zadocs featured-image duplication** — 73 Articles served from only 13 distinct featured images; shared-skeleton defect behind AdSense "low value content" rejections
- **Pinterest pin creation via bridge** — save-from-URL fetches image but selection UI never renders; bridge lacks CDP file-upload. Needs manual pin or CDP route.
- **Reddit / Medium** — logged out in Chrome profile; CEO backfill for these platforms blocked until user logs in.
