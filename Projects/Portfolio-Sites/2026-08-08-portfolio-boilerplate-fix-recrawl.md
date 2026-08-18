# Portfolio-Wide Boilerplate Fix + Sitemap Recrawl (SumZA, Beanel, 5Minutes)

**Date:** 2026-08-08
**Project:** Portfolio of 7 SA domains

## Summary
Following the ZADocs rewrite (2026-08-07), audited the remaining sites with byte-identity hashing. The genuine template-shell (byte-identical boilerplate) problem was confined to 2 sites; fixed all 3 that needed work, verified all 7 clean, and requested recrawls on all 7.

## Audit Results (byte-identity hashing, not word counts)

| Site | Posts | Issue |
|------|-------|-------|
| **beanel.com** | 33 | 28 posts with byte-identical "Why This Matters" block (736–1,315 words each = 40-65% of article). Worst. |
| **sumza.co.za** | 32 | 16 posts with byte-identical 181-word "Why This Matters" block |
| **5minutes.co.za** | 29 | 7 posts slightly under 1,500 words (1,362–1,486) |
| whippetqr.com | 37 | Natural structure, all 1,500+ — clean |
| saymyname.co.za | 28 | Natural structure, all 1,500+ — clean |
| howzitza.co.za | 15 | One "Why This Matters Beyond the Fun" on a single post = legitimate, not a shell |

## Fixes Applied (DONE + verified)

### sumza.co.za (16 posts)
- Removed the byte-identical "Why This Matters for Your Financial Health" block from all 16.
- Added unique top-up sections to 3 that dropped below 1,500 (battery-backup, overtime, APS).
- **Verified: 16/16 clean, all ≥1,500 words.**

### beanel.com (28 posts)
- Authored 28 unique supplemental HTML sections (parallel subagents, all validated clean, no boilerplate).
- Removed the byte-identical "Why This Matters" blocks, appended each unique section.
- **Verified: 28/28 clean, all ≥1,500 words (1,503–1,894).**

### 5minutes.co.za (7 posts)
- Added unique top-up sections to all 7 short posts.
- **Verified: all render 1,500+ words.** (Post 138 was Elementor-rendered; DB confirmed 1,629 words, rendered page 1,506 — the "REST shows 1486" was stale cache.)

## Final Cross-Site State
- **All 7 sites: 0 boilerplate markers, every article 1,500+ words.** Verified via REST + rendered pages.

## Sitemap Resubmission + Recrawl (DONE)
- **IndexNow**: Pinned all 7 sitemaps — all returned HTTP 202 (accepted).
- **Google Search Console** (via user's logged-in Chrome through Chrome Connector bridge): Requested "Request Indexing" recrawl on all 7 sites (whippetqr, howzitza, sumza, zadocs, saymyname, 5minutes, beanel) — all confirmed "REQUESTED".
- **Sitemaps already submitted** in GSC: whippetqr (`sitemap_index.xml`, Success, 55 pages) + sumza (`sitemap_index.xml`, Success, 98 pages). Re-pinged to force fresh read.
- All temporary Chrome tabs closed (only user's pre-existing AdSense tab remains).

## Key Technique (reused from zadocs)
- `wp_update_post` hangs on cp47/beanel for large content → use **direct `$wpdb->update`** on `wp_posts`.
- REST API is read-only (401 for edits) → deploy PHP script via FTP, run, delete.
- Pace requests ~6s apart on cp47 (throttles rapid PHP).
- Byte-identity hashing distinguishes real template-shells from natural article structure.

## Files
- Beaner top-ups: `/home/m/zadocs-wordpress/portfolio-content-fix/beanel-topups/*.html` (28 files)
- Publish scripts: in `/home/m/zadocs-rewrite/` (beanel-boiler-remove.php, sumza-boiler-remove.php, 5min-topup.php) — all deleted from servers after use.

## Git
- Branch: `fix/adsense-content-rewrite` on zadocs-wordpress (portfolio fixes committed here).
