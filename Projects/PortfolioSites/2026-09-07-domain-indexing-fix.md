# 2026-09-07 Domain Indexing Investigation & Fix

## What happened
User reported domains "could not be indexed." Investigation via Google Search Console (Chrome bridge, logged into user's Google) showed all 7 sites UP and crawlable — the real problem was **stale content** plus a **broken cron pipeline**.

## Root cause
1. **Ollama cloud account hit weekly usage limit Sept 4** (`HTTP 429: admiring_jemison_742 reached weekly usage limit`). Old model `qwen3.5:397b` (397B) burned tokens fast, so the weekly blog cron and other crons stopped firing.
2. **Gateway ran with a stale API key** (401) — loaded old key from config before it was updated.

## GSC indexing status (before fix)
| Site | Indexed | Not indexed | Top reasons |
|------|---------|-------------|-------------|
| beanel.com | 37 | 33 | 21 crawled-not, 5 discovered-not |
| sumza.co.za | 93 | 39 | 14 crawled-not, 11 discovered-not, 7×404 |
| zadocs.co.za | 295 | 246 | 93×404, 60 crawled-not, 42 discovered-not |
| howzitza.co.za | 58 | 32 | 13×404, 8 crawled-not |
| whippetqr.com | 54 | 43 | 19 noindex, 11 crawled-not |
| saymyname.co.za | 55 | 41 | 14 crawled-not, 9 discovered-not |
| 5minutes.co.za | 42 | 16 | 7 discovered-not, 4 crawled-not |

Dominant reason everywhere: "Crawled/Discovered – currently not indexed" = stale content. Newest article on all sites was Aug 25.

## Fixes applied
1. **Switched all models to `deepseek-v4-flash`** — main config, 6 cron jobs with explicit models, failover scripts. Cheaper per token than qwen3.5:397b.
2. **Updated config.yaml API key** to working `OLLAMA_API_KEY` from `.env` (old key returned 401).
3. **Restarted gateway** (systemd user service) to pick up working key.
4. **Triggered weekly blog cron** — ran with deepseek-v4-flash, published 7 fresh articles (one per site), all 200 with 2+ images, all in sitemaps, all with lastmod.
5. **Pinged IndexNow** for all 7 new articles (202 OK).

## New articles published (all live, HTTP 200)
- 5minutes.co.za: /classic-card-games-to-play-at-a-south-african-braai/
- beanel.com: /what-is-a-mac-address-and-why-it-matters-for-your-home-network/
- sumza.co.za: /how-to-calculate-your-electricity-bill-in-south-africa/
- zadocs.co.za: /how-to-write-a-last-will-and-testament-in-south-africa/
- howzitza.co.za: /south-african-street-food-flavourful-journey-mzansi/
- whippetqr.com: /2026/09/07/qr-codes-for-restaurant-menus-south-african-guide/
- saymyname.co.za: /beautiful-zulu-baby-names-and-their-meanings/

## Notes
- zadocs 93×404 are historical (old deleted slugs); current sitemap URLs all 200 — will clear as Google re-crawls.
- whippetqr 19 noindex is historical; current sitemap pages all indexable.
- Sitemap lastmod now present on all 7 sites (was missing on 6).
- Re-check GSC in a few days — "crawled but not indexed" should resolve as Google sees active content.
