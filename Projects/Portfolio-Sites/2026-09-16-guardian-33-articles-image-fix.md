# Guardian Run — 2026-09-16 06:05 SAST

**Server: all 7 sites UP (HTTP 200).** No crash, no throttling, no DNS change. beanel.com on its own host (164.160.91.56), 6 on cp47 (164.160.91.40).

## Site health — all verified independently

| Site | Homepage | Posts | Sitemap | Essentials | ads.txt | AdSense meta | -2/-3 slugs |
|------|----------|-------|---------|-----------|---------|--------------|-------------|
| beanel.com | 200 | 40 | 200 | canonical 200 | 200 | 1 | 0 |
| howzitza.co.za | 200 | 34 | 200 | canonical 200 | 200 | 1 | 0 |
| sumza.co.za | 200 | 39 | 200 (`wp-sitemap.xml`) | canonical 200 | 200 | 1 | 0 |
| zadocs.co.za | 200 | 73 | 200 | `/about/`→301 canonical, rest 200 | 200 | 1 | 0 |
| saymyname.co.za | 200 | 34 | 200 | canonical 200 | 200 | 1 | 0 |
| whippetqr.com | 200 | 44 | 200 | canonical 200 | 200 | 1 | 0 |
| 5minutes.co.za | 200 | 36 | 200 | canonical 200 | 200 | 1 | 0 |

All 7 counts identical to the previous cycle — no content change by any external process.

## Verified the prior cycle's open defect — RESOLVED

The 2026-09-15 cycle reported an **Orchid Privacy Network advertisement live as the article image on whippetqr.com post 197**. Verified this run:

- Post 197 `qr-code-marketing-...` now serves `2026/09/wq197-body.jpg`; `post_modified` = 2026-09-15T05:32
- The old ad file `2026/07/qr-code-marketing-business.jpg` returns **HTTP 404** (removed)
- Current image vision-inspected with the full mandatory prompt: clean — an office brainstorming scene, no text/logo/brand/watermark/ad
- Rendered page: 1 figure, 3,046 words, HTTP 200

## Fixed this run — 33 articles had fewer than 2 rendered images

**Root cause:** the howzitza/5minutes/beanel themes do **not** call `the_post_thumbnail()`, so `featured_media` is stored in the DB but never rendered. The guardian counts rendered `<img>` tags, so posts with a featured image set still rendered 0–1 images. This is the documented two-phase pattern — the hero must be embedded in `post_content`.

**5minutes.co.za — 26 articles.** All had `featured_media` set but rendered **0 content images**. Fixed via PHP-over-FTP: prepended the hero figure to `post_content` and inserted a second, different image at ~55% depth (verified depths 0.50–0.67). 21 standard posts by `wp_update_post`; 5 Elementor posts (132/134/136/138/140, content in `_elementor_data`) fixed by mutating the text-editor widget JSON, then clearing `_elementor_files_manager->clear_cache()` + elementor transients + LiteSpeed purge (Elementor served stale output until the cache clear). Post 330 needed a separate pass.

**beanel.com — 7 articles rendering the SAME image twice.** Posts 160/157/155/151/149/147/144 had the identical photo in both figure slots. Replaced the 2nd figure's `src` with a vision-verified, different-subject photo (also stripped the stale `srcset`).

Every candidate image was vision-inspected with the mandatory full-prompt question before deployment. Rejected during inspection: `south-african-braai-fire.jpg` (Tesco branding on a bag), `Men-playing-cards.jpg` (17th-century French engraving with title/credits), a Joburg libraries photo (Joburg logo baked in).

All 7 deployment scripts were deleted after use and confirmed HTTP **404**.

## Independent verification (not the subagent's self-report)

Rendered-page curl on 12 of the fixed URLs — each returns 2 distinct content images, 1,772–2,185 words, no duplicate H2, HTTP 200:

- 5minutes 132, 134, 136, 138, 140, 330, 457, 207, 192 → 2 unique images each
- beanel 160, 157, 155, 151, 149, 147, 144 → 2 unique images each

Post-fix full sweep: **0 of 36 5minutes posts** render fewer than 2 content images (was 26).

## beanel.com map — confirmed working, all 5 guardian flags are FALSE POSITIVES

The guardian flags "CSS min-height fix MISSING", "MutationObserver MISSING", "invalidateSize MISSING", "init script did NOT execute", "IP stuck on Loading...". Verified live as a logged-out visitor:

```
leaflet_container: true    zoom_in: true    zoom_out: true
attribution: true          leaflet_defined: true
ip_text: "102.132.217.18"  tiles_loaded: 8   map_h: 350
cookie_banner: true (accepted)  litespeed_deferred: 0
city: "Mossel Bay"   isp: "Cool Ideas Service Provider (Pty) Ltd"
```

Vision on the screenshot: street map with roads and highways (N1/N3/N12/N14), Johannesburg area, blue location marker centred, zoom +/- top-left, "Leaflet | © OpenStreetMap" attribution bottom-right, no admin bar. **The map is working.** The guardian reads curl source, where JS-rendered markers never appear.

## Remaining — pre-existing, not fixed this run

1. **Cross-post hero reuse.** 5minutes has 7 shared `featured_media` groups (3–5 posts each), sumza 3 groups (13/11/6 posts), zadocs 3 groups (~20 posts each), howzitza 1 group of 2. The media pool per site is too small for 36–73 articles, so heroes repeat. Visible in the new guardian output; needs a fresh-image sourcing pass, not a code fix.
2. **zadocs.co.za — 61 of 73 template pages render 0 content images.** All 61 have `featured_media` set (only 3 distinct IDs reused across them) but the theme does not render it. Largest remaining content gap on the portfolio.
3. **Cross-post in-content image reuse is now present** on 5minutes (each added image is reused by 2–5 posts from the existing pool) — a side effect of sourcing from the site's existing verified images rather than uploading 26 new ones. Distinct within every article, but not globally unique.
4. **howzitza `blog-stereotypes-funny.png`** appears in 4 posts and `blog-braai-oom-group.png` in 3 (pre-existing).

## Guardian script corrections still needed (carried over from 2026-09-15)

- Featured-image sharing check should grep inside `entry-content` only — `logo-group-photo.png`, `sumza-logo.png`, `cropped-Second-Logo.png` are theme header logos, not article images.
- beanel map heuristics should stop flagging JS-rendered markers as missing.
- **New this run:** the script should report "N posts render <2 content images" as a single count, not just featured-media sharing — that is the metric that actually matches what a visitor sees.
