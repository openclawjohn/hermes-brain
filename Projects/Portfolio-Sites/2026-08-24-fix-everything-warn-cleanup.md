# 2026-08-24 Fix Everything — WARN Cleanup Pass Across All 7 Sites

## Date
2026-08-24

## What was done
After the auditor's 4 enrichment courses (7-10) surfaced 56 WARNs across the 7 sites, this session executed a "fix everything" pass. Addressed every real WARN that was fixable at the server/content level; fixed auditor false-positives that were generating noise.

## Completed fixes

### Content fixes
- **Titles trimmed (18 posts):** over-long titles (>70 chars) shortened across sumza (3), zadocs (1), saymyname (3), 5minutes (11). Verified via REST.

### Server/technical fixes
- **Cache-Control headers (all 7 sites):** added via `.htaccess` `<IfModule mod_headers.c>` — HTML `no-cache, must-revalidate`, static assets `public, max-age=31536000, immutable`. Verified live on all 7.
- **Author bylines (beanel, whippetqr):** deployed `portfolio-author-byline` mu-plugin emitting `meta name=author`, `rel=author`, `article:author` (E-E-A-T signal). Verified live.
- **Sitemap lastmod (4 sites):** regenerated static post/page sitemaps WITH `<lastmod>` on beanel (36), zadocs (69), whippetqr (40), 5minutes (32). sumza's native sitemap already fresh. Verified.
- **A11y + perf mu-plugin (`portfolio-a11y-perf`, all 7 sites):** font-size floor (16px), tap-target sizing (44px min), and deferral of non-critical render-blocking scripts. Verified live, pages render OK.
- **Nav aria-labels mu-plugin (`portfolio-aria-labels`, all 7 sites):** server-side `nav_menu_link_attributes` filter + JS fallback for WCAG 4.1.2.
- **WebP image conversion (all 7 sites):** on-server GD converter deployed, converting jpg/png → webp q82. Combined with `portfolio-webp-wrap` mu-plugin that wraps images in `<picture>` with an `image/webp` source. Verified all 7 serve WebP.

### Auditor false-positive fixes (script)
- **`check_security_headers` lowercase-header bug:** HSTS present on all sites but flagged on zadocs because `hdrs.get("Strict-Transport-Security")` is case-sensitive. Fixed to check `strict-transport-security` lowercased.
- **`better_ads_standards` animation false-positive:** the check flagged 5minutes game pages for "flashing/animated ad" due to the site's own `#found-flash` game animation, not an ad. Tightened to only flag animation adjacent to an actual ad unit.

## Verification
- All 7 sites: WebP picture-wrap = YES, a11y CSS = YES, script defer = YES (verified live).
- Cache-Control live on all 7.
- Sitemaps have lastmod on all sites.
- Author bylines live on beanel + whippetqr.
- Full re-audit run (report-fixed.json) in progress to measure WARN reduction.

## Files
- Auditor: `/home/m/.hermes/scripts/adsense-auditor.py`
- New mu-plugins (all sites): `portfolio-author-byline.php`, `portfolio-a11y-perf.php`, `portfolio-aria-labels.php`, `portfolio-webp-wrap.php`
