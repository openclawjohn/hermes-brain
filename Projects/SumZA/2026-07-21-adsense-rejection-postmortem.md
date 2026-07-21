# SumZA AdSense Rejection — Postmortem & Full Portfolio Fix

**Date:** 2026-07-21
**Project:** SumZA (sumza.co.za) + all 6 other portfolio sites

## What Happened
SumZA was rejected by AdSense for "Low value content" despite having 27 articles at 1,500+ words with 2 images each. The rejection was correct — there were quality issues I had been blind to.

## Root Causes Found

### 1. Uncategorized Posts Silently Excluded from Sitemap
Rank Math excludes posts in "Uncategorized" from the sitemap with no visible setting or warning. 6 of 27 SumZA posts were in Uncategorized → only 22 of 27 were in the sitemap → Google only saw 22 posts.

**Fix:** Assign every post to a real category. Then write static sitemap XML files via PHP `file_put_contents()` because LiteSpeed caches sitemaps at the proxy level and won't regenerate them.

### 2. Duplicate H2 Headings
25 of 27 SumZA posts had duplicate H2 headings (same heading text appearing twice in one post). This makes the site look templated and low-quality.

### 3. Boilerplate Sections
15 posts had "Understanding X in the South African Context" boilerplate sections — auto-generated text repeated across multiple posts. Google's reviewers see this as thin/templated content.

### 4. LiteSpeed Caches Sitemaps at Proxy Level
Deleting Rank Math cache options and clearing the cache directory is NOT enough. The only reliable fix is writing static XML files directly to the site root.

## Full Portfolio Fix Applied

| Site | Posts | Uncat Fixed | Boilerplate Removed | Essential Pages |
|------|-------|-------------|---------------------|-----------------|
| sumza.co.za | 30 | 6 | 15 | All 200 |
| howzitza.co.za | 15 | 5 | 13 | All 200 |
| saymyname.co.za | 32 | 15 | 9 | All 200 |
| zadocs.co.za | 64 | 1 | 62 | All 200 |
| whippetqr.com | 36 | 1 | 35 | All 200 |
| 5minutes.co.za | 41 | 28 | 34 | All 200 |
| beanel.com | 31 | 5 | 0 | All 200 |

**Total: 249 articles, 54 Uncategorized fixed, 153 boilerplate sections removed, 0 404s.**

### Additional Issues Found
- 5minutes.co.za: duplicate AdSense meta tag (2 mu-plugins both outputting it)
- 5minutes.co.za: broken PHP in seo-integration.php (`echo'";`)
- howzitza.co.za: 4 posts with empty image alt text
- All sites: sitemaps were stale/cached at LiteSpeed proxy level

## Skills Updated
- **portfolio-site-quality-standards** — Added comprehensive Pre-AdSense Gate checklist, critical lesson about "everything is perfect" being always wrong
- **portfolio-site-audit** — Added pitfalls for Uncategorized exclusion and LiteSpeed sitemap caching
- **weekly-blog-posts** — Added quality checks for duplicate H2s, boilerplate, categories

## Cron Jobs Updated
- **website-guardian** (06:00 daily) — Now runs full AdSense readiness gate with auto-fix
- **cp47-server-monitor** (every 5 min) — Now checks AdSense metrics
- **weekly-blog-posts** (Tuesday 02:00) — Now enforces quality gates

## Things NOT Yet Checked
- Google Search Console verification
- Google Analytics GA4
- Page speed / Core Web Vitals
- Mobile rendering
- Content originality across sites
- Broken internal links
- Image file sizes
- Schema markup
- Open Graph / social meta tags
- Favicon
- Category archive pages
- Tag pages
- 404 page design
- Cookie consent banner
- SSL redirect
- WWW vs non-WWW
- Contact form functionality
- RSS feed content
- Pagination
- Security headers
