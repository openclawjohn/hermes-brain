# Portfolio Monitor — 2026-09-15 08:10 SAST

**Cron:** cp47-server-monitor (5-min). **Regime:** Phase F maintenance mode (single-pass bulk scan).
**Verdict:** No site status change. Housekeeping executed (pending from 07:53 cycle). Sitemap/slug backlogs now clear.

## Site status — all 7 UP, no oscillation

| Site | Home | Articles | Sitemap locs vs REST | ads.txt | AdSense meta | Essentials |
|------|------|----------|---------------------|---------|--------------|------------|
| beanel.com | 200 | 40 | n/a (separate host) | 200 ✅ | 1 | ✅ resolving |
| howzitza.co.za | 200 | 34 | 34 vs 34 ✅ | 200 ✅ | 1 | ✅ 5/5 |
| sumza.co.za | 200 | 39 | 39 vs 39 ✅ (`wp-sitemap.xml`) | 200 ✅ | 1 | ✅ 6/6 |
| zadocs.co.za | 200 | 73 | 73 vs 73 ✅ | 200 ✅ | 1 | ✅ 5/5 |
| saymyname.co.za | 200 | 34 | 34 vs 34 ✅ | 200 ✅ | 1 | ✅ 5/5 |
| whippetqr.com | 200 | 44 | 44 vs 44 ✅ | 200 ✅ | 1 | ✅ 4/4 |
| 5minutes.co.za | 200 | 36 | 36 vs 36 ✅ | 200 ✅ | 1 | ✅ 5/5 |

Total Articles: **300**.

**Sitemap gap = 0 on every site.** Previously flagged Rank Math exclusion gaps (beanel 31/16, sumza 27/22, saymyname 31/17, 5minutes 40/13) are resolved. Content counts rose across sites — the Tuesday 02:00 weekly-articles cron landed (`2026-09-15-weekly-articles-all-7-sites.md`).

**whippetqr sitemap corruption resolved.** `robots.txt` now declares `sitemap_index.xml` (Rank Math), no longer `wp-sitemap.xml` (which returned homepage HTML). `sitemap_index.xml` + sub-sitemaps serve valid XML with correct loc counts. The long-standing "worst-positioned site" defect is closed.

**Broken slugs (-2/-3) = 0 on all 7.** Backlog cleared (prior tally: 5minutes 15, saymyname 5, whippetqr 4, beanel 2, howzitza 2, sumza 2).

## What was fixed this cycle — housekeeping (pending from 07:53)

19 stale artifacts quarantined by FTP **rename** to `/_hermes_quarantine/` (never delete — recoverable). Each verified 404 by HTTP afterwards.

- whippetqr: `hermes_wq_images.php`, `hermes_imgs/` (the 07:53 cycle's explicit first action)
- howzitza: `full_221.txt`, `full_199.txt`, `imgfix_tmp/`, `expand_extra_howzitza.html`
- zadocs: `imgfix_tmp/`, `expand_extra_zadocs.html`
- 5minutes: `imgfix_tmp/`, `expand_extra_5minutes.html`
- public_html (whippetqr root): `imgfix_tmp/`
- saymyname: `expand_extra_saymyname{,3,4}.html`
- sumza: `config-diagnostic-results.txt`, `expand_extra_sumza.html`

**Safety checks before removal, not assumption:**
- Downloaded and read the `.txt` dumps: harmless content/debug output, no secrets (the only "secret" grep hit was article prose — "secret spice blend").
- Confirmed **0 posts reference `imgfix_tmp`** by scanning post content server-side across all 4 affected sites before quarantining the image dirs — removal could not break a live page.
- All 7 homepages re-verified 200 **after** the removals.

## Remaining — content-generation work, not server work

**82 Articles still have no in-body image: zadocs 61, 5minutes 21.** Howzitza (was 4), whippetqr (was 29), saymyname, sumza all verified at **0** this cycle — the prior alarm is fully closed except these two sites.

**0 Articles are missing a featured image** portfolio-wide.

**zadocs shared featured images** — 73 Articles served from only 13 distinct featured images. Same shared-skeleton defect behind the AdSense "low value content" rejections. Not fixable by reuse; needs distinct subject-checked images per Article.

## Method note

All counts taken from REST `x-wp-total` and sitemap `<loc>` parsing, with 18–22s spacing between cp47 requests. No throttling encountered. Content/image analysis done locally on fetched JSON — not by external curl counting, which silently truncates and produces false zero-image alarms.
