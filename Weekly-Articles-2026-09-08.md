# Weekly Articles — 2026-09-08

**Cron:** weekly-blog-posts (b054e0d5026d) · Tue 02:00 SAST
**Result:** 1 article per site, all 7 domains. All 1,500+ words, 2 distinct images each, real categories, static sitemaps regenerated.

## Articles Published

| Site | Article | Words | URL |
|------|---------|-------|-----|
| beanel.com | What Is DNS and How Does It Work in South Africa? | 2,017 | https://beanel.com/what-is-dns-and-how-does-it-work-in-south-africa/ |
| howzitza.co.za | The 11 Official Languages of South Africa: A Complete Guide | 2,162 | https://howzitza.co.za/the-11-official-languages-of-south-africa-a-complete-guide/ |
| sumza.co.za | How to Calculate Your Income Tax in South Africa: A Complete Guide | 2,372 | https://sumza.co.za/how-to-calculate-your-income-tax-in-south-africa-a-complete-guide/ |
| zadocs.co.za | Service Level Agreement in South Africa: A Complete Guide | 2,418 | https://zadocs.co.za/service-level-agreement-in-south-africa-a-complete-guide/ |
| saymyname.co.za | Beautiful Xhosa Baby Names and Their Meanings | 2,322 | https://saymyname.co.za/beautiful-xhosa-baby-names-and-their-meanings/ |
| whippetqr.com | QR Codes for Real Estate in South Africa: A Complete Guide | 2,447 | https://whippetqr.com/2026/09/08/qr-codes-for-real-estate-in-south-africa-a-complete-guide/ |
| 5minutes.co.za | Fun Games to Play at a South African Wedding | 2,390 | https://5minutes.co.za/fun-games-to-play-at-a-south-african-wedding/ |

## Quality Gates — All Passed

- **2 distinct images per article** — featured at top (h2_before=0), in-content at ~55% (h2_before 3-5). Different subjects, never stacked, never same file.
- **1,500+ words** body text each (verified on rendered page, stripped HTML).
- **No duplicate H2 headings** — fixed 6 mid-article duplicate "Conclusion" sections + whippetqr duplicate "Best Practices" after expansion.
- **No boilerplate sections** — article bodies clean; no repeated sentences 3+ times.
- **Real categories** — none in Uncategorized (Rank Math sitemap exclusion avoided).
- **HTTP 200** on all 7 live pages (logged-out curl).
- **In sitemaps** — all 7 new URLs present in regenerated static `post-sitemap.xml`.
- **Nav labels** — all 7 sites say "Articles", no "Blog".

## Deployment Method

PHP scripts via FTP (proven reliable; REST throttles). Images generated with fal.ai Klein (`flux-2/klein/9b`), strict "no text no writing no labels" prompts. cp47 sites via `cp47-jhb.za-dns.com` (whippetq), beanel on separate server (`www.clashofthecultivars.com`). Sitemaps rewritten as static XML to bypass LiteSpeed cache. All temp scripts deleted after execution.

## Issues Encountered & Fixed

- **Initial word counts below 1,500** (941-1,157w) — expanded all 6 cp47 articles with substantial additional sections.
- **Duplicate "Conclusion" H2s** after expansion — removed mid-article duplicates on all 6 cp47 sites.
- **whippetqr duplicate featured image** — theme rendered featured image + content embedded it. Removed embedded duplicate from content.
- **Sitemaps stale** (LiteSpeed cache) — regenerated static `post-sitemap.xml` on all 7 sites.

## Note on Image Inspection

`vision_analyze`/browser-visual tools not available in this cron environment. Used validated Klein workflow with strict anti-gibberish prompts (produced clean images in prior weekly batches). Recommend visual spot-check of featured images on next desktop session per mandatory inspection protocol.

## Pre-existing Issue (not introduced this week)

All sites have a theme/SEO-plugin auto-generated FAQ schema (JSON-LD) injected on every post with templated Q&A text ("The guide above walks through it step by step..."). This is site-wide boilerplate, not in article content. Flagged as a potential low-value-content concern for AdSense review — worth addressing separately.
