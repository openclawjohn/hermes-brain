# 2026-09-07 Image Fix + Full Portfolio Audit

## Context
User asked to fix the 9 failing images on the 7 new articles (published earlier today by the weekly blog cron) and run a full audit of all 7 sites.

## Image audit findings (14 images inspected via vision_analyze)
7 of 14 images failed the no-text/no-brand/no-AI-artifact rule:
- **5minutes**: featured was a historical engraving with French text + artist credits
- **beanel**: router had Japanese text + "AX4 3200 WSR Series" brand; network had port numbers
- **zadocs**: both images were full of handwritten will text (one was Victor Hugo's actual will with "ARCHIVES NATIONALES" stamp)
- **howzitza**: food photo was blurry/overexposed; market had "tomatoes" label + "Durex" brand on crate
- **whippetqr**: sign had logo+text+QR; "QR Code" image was actually a black square painting (irrelevant)

## Fix approach
- Generated replacement images via fal.ai Klein (flux-2/klein/9b) — clean, text-free, site-appropriate
- 3 images had AI gibberish on first pass (network cables, food, market) — regenerated with tighter prompts
- Playing cards image: AI kept generating gibberish card ranks → used real Wikimedia Commons photo (Carpet patience 2.jpg) instead
- Uploaded via FTP to each site, imported into media library via PHP script, set as featured + inserted in-content at ~55%

## Key pitfall discovered
**PHP `wp_insert_attachment` with a file outside the uploads dir produces a broken URL** (`/wp-content/uploads//home/...`). Must `copy()` the file into `wp_upload_dir()['path']` FIRST, then insert. Also, the first broken run stripped the old in-content image URLs, so a second pass was needed to re-insert them.

## Full audit results (all 7 sites)
| Check | Result |
|-------|--------|
| Homepage | All 7 HTTP 200 |
| Sitemap index | All 7 HTTP 200 |
| ads.txt | All 7 HTTP 200, valid pub ID |
| AdSense meta | All 7 count=1 |
| Essential pages | All 7 have working about/contact/privacy/terms |
| Content counts | REST = sitemap exactly (no Rank Math exclusion gap) |
| Broken slugs | 0 on all 7 |
| Articles nav | All 7 have "Articles" (fixed saymyname "Blog" → "Articles") |

## Fixes applied this session
1. Replaced 9 failing images with clean, relevant, text-free versions (5 sites)
2. Fixed saymyname nav: "Blog" → "Articles", privacy-policy-2 → privacy-policy (hardcoded in astra-child functions.php)

## Remaining
- None blocking. All sites healthy, all images clean, all content indexed-ready.
