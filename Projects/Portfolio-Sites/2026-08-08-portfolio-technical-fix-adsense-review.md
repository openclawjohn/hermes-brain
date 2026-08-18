# Portfolio Technical Fix + AdSense Review (2026-08-08)

**Project:** Portfolio of 7 SA domains

## What Was Done (all verified live)

### AdSense / Search Console
1. **Confirmed all 7 domains verified in Google Search Console** (loaded each property's Performance dashboard — all real).
2. **Submitted site review for zadocs.co.za** — after the content rewrite, clicked "I confirm I have fixed the issues" + "Request review". Status changed from "Needs attention / Low value content" to **"Getting ready / Review requested"**.
3. **Re-uploaded ads.txt on 6 sites** (all on cp47 + beanel) to bump `last-modified` timestamps — forces Google's crawler to re-read ads.txt (6 showed "Not found" despite HTTP 200 + correct content; only saymyname "Authorized").

### Technical Audit + Fixes (never-before-checked items)
| Item | Before | After |
|------|--------|-------|
| **Favicon link** (whippetqr, zadocs) | Missing `<link rel="icon">` in head | ✅ Added via `favicon-injector.php` mu-plugin (files existed, just not referenced) |
| **HTTP→HTTPS redirect** (5minutes.co.za) | Served HTTP 200 (duplicate content risk) | ✅ 301 → HTTPS via .htaccess rule |
| **Security headers** (all 7) | Mostly missing | ✅ HSTS + X-Content-Type-Options + X-Frame-Options + Referrer-Policy + Permissions-Policy via `security-headers.php` mu-plugin |
| **Image lazy-loading** (all 7) | No lazy-load on content images (heavy 564–851KB files) | ✅ Added native lazy-load via `content-image-opt.php` (JS-based, skips hero/LCP image) |

### Audit items verified CLEAN (no action needed)
- OG tags, Twitter cards, Schema/JSON-LD, canonical, title, meta description — all present on all 7
- RSS feed: all 200
- Pagination `/page/2/`: all 200
- 404 pages: all proper (Page Not Found)
- HTTP→HTTPS redirects: all 7 now correct (5minutes fixed)
- www redirects: all 301
- Mobile viewport: all present
- Broken internal links: 0 found
- Essential pages: all 200
- 2+ images per article: all confirmed

### Remaining (larger project, not done)
- **Image file sizes** (564–851KB) — heavy, hurts Core Web Vitals. Would need image optimization/compression (large separate task, not attempted to avoid risk).

## Files Deployed (all sites)
- `security-headers.php` — all 7 sites
- `content-image-opt.php` — all 7 sites
- `favicon-injector.php` — whippetqr, zadocs
- `.htaccess` HTTPS redirect — 5minutes.co.za (backed up to `/tmp/5min-htaccess.bak`)

## Visual Verification
- browser_vision on zadocs employment-contract-template: renders correctly, no visual regression after lazy-load/security changes.

## Git
- Saved mu-plugins to `/home/m/zadocs-wordpress/portfolio-content-fix/`
- Branch: `fix/adsense-content-rewrite`
