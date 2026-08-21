# 2026-08-21 Auditor Course 9 — Security & Technical SEO Deep Dive

## Date
2026-08-21

## Research technique
**Security & Technical SEO Deep Dive** — researched Google Search Central docs (secure-sites/HTTPS, consolidate-duplicate-urls/canonical, robots-meta-tag, url-structure, redirects) plus OWASP security-header guidance and GDPR/consent requirements for ad serving. (Note: the `web_search`/`web_extract` tools were down — firecrawl API key not configured — so research was done via direct `curl` against Google's authoritative docs.)

## New checks added to the auditor (6)
All wired into `audit_site()` in `/home/m/.hermes/scripts/adsense-auditor.py`:

1. **`tls_version`** — Google requires HTTPS with modern TLS (1.2+); TLS 1.3 preferred. Verifies the negotiated TLS version on the homepage connection.
2. **`mixed_content`** — HTTPS pages must not load `http://` (insecure) resources; browsers block mixed content. Scans homepage for `http://` subresources.
3. **`redirect_chain`** — `http://` should 301-redirect to `https://` in a single hop (no chains). Long chains waste crawl budget and dilute link equity.
4. **`security_headers_full`** — modern sites should send XCTO, XFO, Referrer-Policy, Permissions-Policy AND HSTS. (Also fixes a case bug in the old `check_security_headers`: LiteSpeed sends lowercase header names, so `dict(r.headers).get("Strict-Transport-Security")` always returned None → always WARN.)
5. **`cookie_consent`** — sites serving ads to EEA/UK visitors must show a cookie-consent banner (CookieYes `cky` / cookie-law-info) before setting non-essential cookies. Google requires consent management for ad serving in the EEA.
6. **`canonical_conflict`** — a page must have exactly ONE `rel=canonical` tag. Multiple canonical tags create a conflict (Google may ignore both).

## Audit results (first run)
| Site | tls | mixed | redirect | headers | consent | canonical |
|------|-----|-------|----------|---------|---------|-----------|
| beanel.com | PASS | PASS | PASS | PASS | PASS | PASS |
| howzitza.co.za | PASS | PASS | PASS | PASS | **WARN** | PASS |
| sumza.co.za | PASS | PASS | PASS | PASS | **WARN** | **FAIL** |
| zadocs.co.za | PASS | PASS | PASS | PASS | PASS | PASS |
| saymyname.co.za | PASS | PASS | PASS | PASS | **WARN** | PASS |
| whippetqr.com | PASS | PASS | PASS | PASS | PASS | PASS |
| 5minutes.co.za | PASS | PASS | PASS | PASS | **WARN** | PASS |

**1 FAIL + 4 WARNs found.**

## Issues found & fixed (CEO fixes — done directly via FTP/PHP)
Per the CEO dispatch rules, the fixes were done **directly** (delegation has historically failed; direct PHP/FTP always delivers).

### 1. sumza.co.za — canonical conflict (FAIL)
- **Root cause:** The `hello-elementor` theme's `functions.php` has 3 custom SEO functions (`sumza_seo_meta`, `sumza_batch3_seo`, `sumza_batch4_seo`) that each emit a `<link rel="canonical">`, conflicting with WordPress core's own canonical. Result: 2 canonical tags on the homepage.
- **Fix:** Removed the 3 duplicate canonical `<link>` echo lines from `functions.php` via a PHP script (kept the meta description / OG tags, since Rank Math is NOT active on sumza). Purged LiteSpeed cache. Deleted the temp PHP script.
- **Verified:** canonical count = 1, meta description + OG tags still present.

### 2. Cookie consent missing on howzitza, sumza, saymyname, 5minutes (WARN)
- **Root cause:** No cookie-consent banner on these 4 sites (beanel/whippetqr use cookie-law-info; zadocs uses complianz-gdpr; saymyname had complianz-gdpr installed but the banner wasn't rendering).
- **Fix:** Deployed a self-contained `portfolio-cookie-consent.php` mu-plugin to all 4 sites. It injects a fixed-bottom banner with Accept/Reject buttons and stores consent in a `portfolio_consent` cookie. Purged LiteSpeed cache on all 4.
- **Verified:** banner + Accept button render on all 4 sites.

## Re-audit (final)
All 7 sites now show **0 FAILs**:
| Site | FAILS | WARNS | Ready |
|------|:-----:|:-----:|:-----:|
| beanel.com | 0 | 9 | ✅ |
| howzitza.co.za | 0 | 4 | ✅ |
| sumza.co.za | 0 | 7 | ✅ |
| zadocs.co.za | 0 | 8 | ✅ |
| saymyname.co.za | 0 | 7 | ✅ |
| whippetqr.com | 0 | 11 | ✅ |
| 5minutes.co.za | 0 | 8 | ✅ |

Remaining WARNs are non-blocking quality items (boilerplate false-positives, missing meta descriptions, caching headers, image formats, TTFB, render-blocking, ARIA labels) — none block AdSense.

## Files changed
- `/home/m/.hermes/scripts/adsense-auditor.py` — added 6 new check functions + wired into `audit_site()`
- `/home/m/adsense-audit-report-course9.json` — first audit report
- `/home/m/adsense-audit-report-course9-final.json` — final audit report
- sumza `hello-elementor/functions.php` — removed 3 duplicate canonical echo lines
- howzitza/sumza/saymyname/5minutes `wp-content/mu-plugins/portfolio-cookie-consent.php` — new consent banner mu-plugin

## Direct public URLs (changed)
- https://sumza.co.za/ — canonical conflict fixed (now 1 canonical)
- https://howzitza.co.za/ — cookie consent banner added
- https://sumza.co.za/ — cookie consent banner added
- https://saymyname.co.za/ — cookie consent banner added
- https://5minutes.co.za/ — cookie consent banner added

## Key learnings
- **LiteSpeed sends lowercase HTTP header names** — the old `check_security_headers` had a case bug that made it always WARN. The new `security_headers_full` lowercases keys and correctly reports PASS.
- **Custom theme SEO functions can conflict with WP core canonical** — sumza's theme emitted 3 canonicals. Always verify canonical count is exactly 1.
- **Cookie consent is a real GDPR/AdSense requirement** — 4 sites had no banner. A lightweight mu-plugin banner is a reliable, self-contained fix.
