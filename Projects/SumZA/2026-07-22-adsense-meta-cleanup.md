# AdSense Meta Tag Cleanup — All 7 Sites

**Date:** 2026-07-22
**Project:** Portfolio-wide

## What Was Wrong

### 1. Duplicate AdSense Meta Tags
Multiple sites had 2+ `<meta name="google-adsense-account">` tags on the homepage. Google sees this as a technical error.

| Site | Before | Source of Duplicate | Fix |
|------|--------|-------------------|-----|
| **sumza.co.za** | 2 | Old `google-verification.php` mu-plugin + new `site-verification.php` | Deleted old plugin |
| **howzitza.co.za** | 10 | astra-child `functions.php` had 10 `echo` statements | Rewrote clean functions.php |
| **saymyname.co.za** | 2 | astra-child `functions.php` had 1 echo | Removed from functions.php |
| **5minutes.co.za** | 2 | `seo-integration.php` + `site-verification.php` both outputting | Removed from seo-integration.php |
| **beanel.com** | 2 | Theme file embedded meta tag | Removed from theme |

### 2. Broken PHP from Meta Tag Removal
When removing the 10 `echo` statements from howzitza's `functions.php`, orphaned `'";` string literals were left behind, crashing the entire site (500 error). Had to rewrite the entire functions.php from scratch.

### 3. Missing Search Console Verification
5 of 7 sites had no `google-site-verification` meta tag. Without Search Console verification, Google won't crawl ads.txt or accept sitemap submissions.

### 4. ads.txt Missing Trailing Newline
All 7 ads.txt files were missing the required trailing `\n`. Google's ads.txt crawler requires it.

## Fixes Applied
- Created `site-verification.php` mu-plugin for all 7 sites (Search Console + AdSense meta + AdSense script)
- Removed old `google-verification.php` from sumza
- Rewrote howzitza's `functions.php` (clean version preserving all functionality)
- Removed AdSense meta from 5minutes `seo-integration.php`
- Removed embedded meta from beanel theme files
- Re-uploaded all 7 ads.txt files with trailing newline
- Purged LiteSpeed cache on all sites

## Current State
All 7 sites: 1 AdSense meta tag, 1 Search Console meta tag, ads.txt 200 with trailing newline.

## Lessons
1. **Removing PHP echo statements can leave orphaned string literals** — always verify the site still returns HTTP 200 after editing PHP files
2. **LiteSpeed caches the old version** — even after fixing the source, the cached page may still show duplicates. Use cache-busting query params to verify.
3. **Search Console verification is a prerequisite** for Google to crawl ads.txt and sitemaps
4. **ads.txt must end with `\n`** — Google's crawler requires it
