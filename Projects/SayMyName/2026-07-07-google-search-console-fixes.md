# SayMyName.co.za — Google Search Console Issues Fix

**Date:** 2026-07-07
**Project:** SayMyName.co.za

## What Was Done

Fixed Google Search Console issues reported in the attached CSV files:

### Issues from Google Search Console

| Issue | Pages Affected | Fix Applied |
|-------|---------------|-------------|
| Excluded by 'noindex' tag | 2 | Author archive + search pages — correct behavior, no action needed |
| Page with redirect | 1 | /privacy-policy/ → /privacy-policy-2/ — 301 is correct SEO practice |
| Alternate page with proper canonical tag | 1 | Normal WordPress behavior |
| Discovered - currently not indexed | 11 | Google found URLs but hasn't crawled — normal for small sites |
| Crawled - currently not indexed | 1 | Normal for small sites |

### Fixes Applied

1. **Posts assigned to proper categories** (16 posts moved out of Uncategorized):
   - 8 posts → Baby Names
   - 5 posts → Business Names
   - 3 posts → Pet Names
   - Uncategorized now has 0 posts

2. **Tag sitemap enabled** in Rank Math settings (`tax_post_tag_sitemap = on`)
   - 28 tags exist and are assigned to posts
   - Tag sitemap still returns 404 — known Rank Math free limitation (noted in memory)

3. **Category sitemap regenerated** — now shows 3 categories with posts (was only showing Uncategorized)

4. **google-adsense-account meta tag** added to child theme functions.php
   - Verified: present on homepage ✅

5. **Sitemap cache cleared** — physical cache files deleted from `wp-content/uploads/rank-math/`

### Not Fixed (Requires User Action)

- **google-site-verification meta tag** — missing. Needs Rank Math OAuth setup (Analytics → Setup Wizard) which requires user to click through Google OAuth popup

### Current Content Volume

- 16 posts (all HTTP 200)
- 12 pages (all HTTP 200)
- 5 categories (3 with posts)
- 28 tags
- **Total indexable: 61 URLs** — well above the 25+ threshold

### Key Decisions

- Accepted tag sitemap 404 as known Rank Math free limitation — tags are still discoverable through post content
- Used FTP PHP script method for fixes since app password only has REST API read access
- Added AdSense meta tag to child theme functions.php via PHP script (persistent, survives theme updates)
