# 2026-09-07 AdSense "Low Value Content" Fix

## Context
User shared the AdSense Sites dashboard screenshot showing 3 sites flagged "Needs attention — Low value content":
- saymyname.co.za
- 5minutes.co.za
- beanel.com

Also: 5 sites showed ads.txt "Not found" in AdSense, but all 7 ads.txt files are actually correct (HTTP 200, valid pub ID) — that's AdSense crawl lag, not a real problem.

## Root cause of "Low value content"
Audited all 3 rejected sites for the AdSense quality triggers:
- **beanel.com**: 4 FAQ questions repeated VERBATIM across 3 posts each (posts 900, 101, 99). Identical Q&A blocks = templated/duplicated content = textbook "low value."
- **5minutes.co.za**: minor — one repeated CTA footer sentence ("For more South African games, visit 5minutes.co.za").
- **saymyname.co.za**: clean, no repeated content.

## Fix applied (beanel.com)
1. Removed the duplicated FAQ block from posts 900, 101, 99 (PHP via FTP).
2. This dropped them below 1,500 words (1093, 1425, 1325), so expanded each with ORIGINAL, article-specific content:
   - Post 900 (Privacy on Social Media): platform-specific privacy settings, scam spotting, account recovery → 1710w
   - Post 101 (IPv4 vs IPv6): SA transition, home network differences, security → 1912w
   - Post 99 (Free Privacy Tools): password managers, free VPNs, browser extensions → 1880w
3. Verified: all 3 posts 1,700+ words, no repeated FAQ, no duplicate H2s, live HTTP 200.

## Audit gate updated
Added "Low Value Content Triggers" section to the post-cron-audit-gate skill:
- No repeated FAQ blocks across posts
- No repeated sentences/paragraphs (sentence-frequency scan, flag 3+)
- No templated CTA footers
- No overused boilerplate phrases
- Content must be article-specific, not recycled
- Detection + fix methods documented

## What's left
- User must click "Request review" in AdSense for the 3 sites (requires Google login — I can't do this).
- 5minutes CTA footer is minor; could vary wording per post but not critical.
- ads.txt "Not found" in AdSense will clear on re-crawl (files are correct).
