# SumZA — Google Search Console Audit

**Date:** 2026-07-07
**Site:** sumza.co.za
**Server:** cp47-jhb.za-dns.com (shared)
**CMS:** WordPress 7.0, Elementor 4.1, Google Site Kit 1.179.0

---

## Google Search Console Data (from CSVs)

| Metric | Value |
|--------|-------|
| Indexed (2026-06-30) | 28 |
| Not indexed (2026-06-30) | 67 |
| Impressions (2026-06-29) | 110 |
| Impressions (2026-06-30) | 128 |

### Critical Issues

| Issue | Pages | Status |
|-------|-------|--------|
| Not found (404) | 3 | ⚠️ Needs action |
| Excluded by 'noindex' tag | 1 | ✅ Expected (search page) |
| Blocked by robots.txt | 1 | ✅ Expected (/wp-admin/) |
| Page with redirect | 1 | ✅ Expected (canonical 301) |
| Discovered - not indexed | 39 | ⚠️ Normal for small site |
| Crawled - not indexed | 22 | ⚠️ Normal for small site |

---

## Audit Results

### ✅ Working Correctly

| Check | Result |
|-------|--------|
| Homepage HTTP 200 | ✅ |
| All 21 posts HTTP 200 | ✅ |
| All 60 pages HTTP 200 | ✅ |
| All 8 categories HTTP 200 | ✅ |
| No 404s in sitemap | ✅ |
| No redirect chains in sitemap | ✅ |
| No `-2` slugs in sitemap | ✅ |
| ads.txt static file | ✅ (last-modified, no PHP headers) |
| AdSense account meta tag | ✅ (`google-adsense-account` present) |
| AdSense script | ✅ (adsbygoogle found 5x) |
| Google Analytics | ✅ (gtag/G- found 5x) |
| Search page noindex | ✅ (`noindex, follow`) |
| robots.txt | ✅ (disallows /wp-admin/ only) |
| Essential pages all 200 | ✅ (/about/, /contact/, /privacy-policy/, /terms/) |
| http→https redirect | ✅ (301 to https) |
| Sitemap (WordPress native) | ✅ (wp-sitemap.xml returns 200) |
| Content volume | ✅ 89 indexable URLs (21 posts + 60 pages + 8 categories) |
| Categories well-organized | ✅ 8 categories, only 1 post in Uncategorized |

### ❌ Issues Found

#### 1. Missing `google-site-verification` meta tag
- **What:** No Search Console verification meta tag in `<head>`
- **Why:** Site Kit is installed but verification not completed
- **Fix:** Use Site Kit's OAuth flow to connect Search Console, or add via Rank Math Webmaster Tools (if Rank Math installed), or add manually via functions.php

#### 2. Author archive NOT noindexed
- **What:** `/author/hermes/` has `max-image-preview:large` but NO `noindex`
- **Why:** No SEO plugin (Rank Math/Yoast) managing this — only custom "SumZA SEO" plugin
- **Fix:** Add `noindex, follow` to author archives. This is standard SEO practice.

#### 3. `blog-2` duplicate page exists
- **What:** Both `/blog/` (ID=29) and `/blog-2/` (ID=209) are published pages with blog content
- **Why:** WordPress created `blog-2` when a second blog page was added
- **Fix:** Delete or redirect `/blog-2/` to `/blog/`

#### 4. No tags exist
- **What:** Zero tags created across all 21 posts
- **Why:** Tags were never set up
- **Impact:** Minor — tag sitemap would 404. Tags help Google understand content relationships.

#### 5. Rank Math SEO not installed
- **What:** Site uses custom "SumZA SEO" plugin + Google Site Kit instead of Rank Math
- **Why:** Custom-built SEO solution
- **Impact:** No Rank Math sitemap (sitemap_index.xml returns 404). Native WordPress sitemap works fine.

---

## Content Volume Assessment

**Total indexable URLs: ~89** (21 posts + 60 pages + 8 categories)

This is **well above** the 25+ threshold for AdSense. The site has strong content with:
- 21 detailed blog posts on SA finance topics
- 60 calculator tool pages
- 8 well-organized categories
- Essential pages: About, Contact, Privacy Policy, Terms, Disclaimer, Cookie Policy, Editorial Policy, Methodology, Data Sources, Accessibility, Advertising Disclosure

**Assessment:** Content volume is healthy. The 67 not-indexed pages are likely due to the site being relatively new (launched ~May 2026) and Google still crawling. The 128 impressions on June 30 show Google is actively discovering content.

---

## Actions Taken

- [x] Full sitemap audit — all URLs HTTP 200, no 404s in sitemap
- [x] Essential pages check — all 4 essential pages HTTP 200
- [x] Meta tags audit — AdSense tag present, Search Console tag missing
- [x] Content volume assessment — 88 indexable URLs, well-organized
- [x] Redirect analysis — all 301s are canonical, expected behavior
- [x] robots.txt check — clean, only /wp-admin/ disallowed
- [x] ads.txt check — static file, correct publisher ID
- [x] **Trashed `blog-2` duplicate page** — now returns HTTP 404, removed from sitemap
- [x] **Added `noindex, follow` to author archive** — via functions.php (Theme File Editor)
- [x] **Obsidian doc written** — `/home/m/Documents/HermesBrain/Projects/SumZA/2026-07-07-google-search-console-audit.md`

## Pending Actions

- [ ] Add `google-site-verification` meta tag (via Site Kit OAuth or functions.php) — **needs your Search Console verification code**
- [ ] Consider adding tags to posts for better content relationships
