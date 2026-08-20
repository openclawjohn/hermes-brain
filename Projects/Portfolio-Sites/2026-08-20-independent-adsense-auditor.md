# Independent AdSense Auditor — Build & First Run

**Date:** 2026-08-20
**Project:** Portfolio-Sites (all 7 domains)
**Status:** Auditor built, first full run complete, findings documented

## What Was Built

A standalone, from-scratch **independent AdSense auditor** at `~/.hermes/scripts/adsense-auditor.py` (backed up to `github.com/openclawjohn/hermes-backup` branch `feature/adsense-auditor`).

It deliberately does **NOT** trust any prior "clean" status. Every AdSense gate item is re-checked independently against the live site. This is the tool the user asked for — an auditor that looks at all our sites and sees if everything is correct for AdSense, without inheriting the assumptions of previous sessions.

### Design rules baked in (from failure history)
1. **Per-site isolation** — one site failing never crashes the run.
2. **Global timeout + per-request timeouts** — no hangs.
3. **Pacing on the old server** (164.160.91.40) — 2s between requests to avoid LiteSpeed throttle.
4. **The decisive template-shell test hashes STRIPPED PLAIN TEXT of FULL sections** — not raw HTML, not truncated prefixes. This is the exact fix that caught saymyname's false "Clean" status on 2026-08-08/18.
5. **Word counts are BODY TEXT ONLY** (strip HTML), never page HTML.
6. **Image counts come from RENDERED pages**, not just REST `featured_media`.
7. **Output:** JSON report + readable per-site summary.

### Checks performed per site
- **Access:** homepage 200, real subpage 200 (not just cached homepage), sitemap index 200, robots.txt
- **Sitemap:** post sitemap URLs, REST `x-wp-total` vs sitemap count (gap), sampled URLs return 200, no `-2`/`-3` slug duplicates
- **Essential pages:** about/contact/privacy/terms all variants return 200 (not 301/404)
- **AdSense technical:** ads.txt (200, correct pub ID, trailing newline), `google-adsense-account` meta count = 1, no noindex, GA4, canonical, OG, favicon, HSTS, HTTP→HTTPS redirect
- **Content quality (REST):** post count (30+), word floor (1,500+ body words), duplicate H2s, boilerplate markers, **byte-identical template shells** (stripped-text full-section hash), Uncategorized posts
- **Images:** rendered pages sampled, 2+ distinct images per post

## First Run Results (2026-08-20, 154s)

| Site | Posts | FAILS | WARNS | Key findings |
|------|:-----:|:-----:|:-----:|-------------|
| beanel.com | 36 | 1 | 1 | 2 posts <2 images; no HSTS |
| howzitza.co.za | 18 | 5 | 1 | **only 18 posts (need 30+)**; 1 dup H2; 1 template shell; 1 Uncategorized; 3 posts <2 images |
| sumza.co.za | 35 | 2 | 0 | 3 dup H2s; **4 template shells** (incl. "Common Misconceptions" ×13, "Practical Next Steps" ×5) |
| zadocs.co.za | 69 | 1 | 1 | 3 posts <2 images; 7 posts boilerplate markers |
| saymyname.co.za | 29 | 3 | 1 | **only 29 posts (need 30+)**; **4 posts under 1,500 words**; 1 post <2 images |
| whippetqr.com | 40 | 3 | 1 | **13 dup H2s**; **2 template shells** ("FAQ About QR Codes" ×6 = 971w identical, "Practical Tips" ×3); 3 posts <2 images |
| 5minutes.co.za | 32 | 2 | 1 | **3 template shells** ("Educational Value of Quick Games" ×16 = 436w identical, "Why These Quick Games Matter" ×16, "Making the Most of Your Game Time" ×6); 3 posts <2 images |

**None of the 7 sites is fully AdSense-ready.** The prior "ALL SITES CLEAN" status (2026-08-08) was wrong — the independent auditor found real, verifiable issues.

## Verified Findings (not false positives)

- **5minutes template shells CONFIRMED:** 16 posts share a byte-identical 436-word "The Educational Value of Quick Games" section (1 distinct SHA1 hash across sampled posts). This is the exact template-shell pattern that triggers "Low value content" rejection.
- **Post counts confirmed via REST `x-wp-total`:** howzitza 18, saymyname 29 (both under the 30-post AdSense floor).

## Key Insight

The 2026-08-08 "ALL SITES CLEAN" claim was based on the same flawed methodology the saymyname fix already exposed: hashing raw HTML (which always differs due to image tags/whitespace) instead of stripped plain text. The independent auditor uses the corrected stripped-text full-section hash and immediately surfaced template shells on 5minutes, sumza, whippetqr, and howzitza that the prior audit missed.

## Next Steps (not done this session — audit only, per user request)

1. **5minutes:** remove/rewrite the 3 byte-identical template sections (16+16+6 posts affected).
2. **sumza:** remove/rewrite 4 template shells ("Common Misconceptions" ×13, "Practical Next Steps" ×5, "Summary" ×5, "UIF Claims Process" ×2) + fix 3 duplicate H2s.
3. **whippetqr:** fix 13 duplicate H2s + 2 template shells ("FAQ About QR Codes" ×6, "Practical Tips" ×3).
4. **howzitza:** add 12+ articles to reach 30; fix 1 dup H2, 1 template shell, 1 Uncategorized post, 3 image-less posts.
5. **saymyname:** add 1+ article to reach 30; expand 4 posts under 1,500 words; fix 1 image-less post.
6. **beanel/zadocs:** add missing images to the flagged posts.
7. Re-run the auditor after fixes to confirm 0 FAILs.

## Files
- Auditor: `~/.hermes/scripts/adsense-auditor.py`
- Report: `/home/m/adsense-audit-report.json`
- Git: `github.com/openclawjohn/hermes-backup` branch `feature/adsense-auditor`
