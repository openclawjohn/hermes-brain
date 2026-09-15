# Portfolio Monitor — 2026-09-15 06:43 SAST

**Cycle type:** Phase F maintenance (single-pass + deep checks)
**Server status:** unchanged — all 7 sites 200, zero oscillation, zero regressions.

## Status table

| Site | Homepage | REST posts | Post-sitemap `<loc>` | Parity | ads.txt | AdSense meta | Essentials |
|---|---|---|---|---|---|---|---|
| beanel.com | 200 ×3 | 40 | 40 | 1:1 ✅ | 200 | 1 | ✅ |
| howzitza.co.za | 200 ×3 | 34 | 34 | 1:1 ✅ | 200 | 1 | ✅ |
| sumza.co.za | 200 ×3 | 39 | 39 | 1:1 ✅ | 200 | 1 | ✅ |
| zadocs.co.za | 200 ×3 | 73 | 73 | 1:1 ✅ | 200 | 1 | ✅ |
| saymyname.co.za | 200 ×3 | 34 | 34 | 1:1 ✅ | 200 | 1 | ✅ |
| whippetqr.com | 200 ×3 | 44 | 44 | 1:1 ✅ | 200 | 1 | ✅ |
| 5minutes.co.za | 200 ×3 | 36 | 36 | 1:1 ✅ | 200 | 1 | ✅ |

Zero `-2`/`-3` slugs · zero Uncategorized · all essential-page 301s resolve to the
canonical (verified by reading the `location:` header on every redirect).

**No status change vs the 06:17 cycle. No server work was required.**

## Work done this cycle: howzitza.co.za image gaps CLOSED

The one open AdSense-gate defect carried over from the 06:17 cycle was 115 Articles
portfolio-wide with no image inside the body. I closed the howzitza portion of it
(a complete site, rather than a partial slice across four).

### Root cause (confirmed, not assumed)

Two independent mechanisms:

1. **Posts written without an in-content image.** IDs 221, 301, 319, 327 had
   image-free `post_content`.
2. **howzitza's theme does not render the featured image at all.** Proven on a live
   logged-out fetch: post 221 had `featured_media` set (thumb 249) yet the rendered
   page contained **0 `<img>` tags**. So a featured image alone buys a howzitza
   Article nothing — only in-body images reach the visitor.

This is the same class of defect documented on the 06:17 cycle, and it is why the
earlier "13 Articles with no featured image" alarm was misleading for this site.

### Fixes applied (all via FTP-uploaded PHP, server-side)

| Post | Slug | Action |
|---|---|---|
| 221 | south-african-personality-types | +2 in-body images (249 braai-oom, 250 jozi-hustler) |
| 301 | south-african-slang…-mzansi-local-knows | +2 in-body images (310 slang-friends-talking, 309 slang-braai) |
| 319 | south-african-braai…-traditions-meat-and-tips | +2 in-body images (317 sa-braai-scene, 318 sa-food-drink) |
| 327 | south-african-street-food-from-kota-to-bunny-chow | +2 NEW Commons photos (bunny chow, Soweto market cart) |
| 342 | south-african-public-holidays… | +1 in-body image (252 scenario-braai-day-match) |
| 413 | south-african-slang-guide-mzansi | featured thumbnail assigned (411) |
| 424 | south-african-rugby… | featured thumbnail assigned (390) |

**All 10 images were visually vetted with `vision_analyze` before deployment** using
the full pixel-scan question. Rejected candidates and why:

- `Market-place-in-Soweto-scaled.jpg` — "Durex" branding + "tomatoes" text on crates
- `South-African-mix-food.jpg` — blurry, grainy, overexposed
- `cape-town-hipster-1.png` — brand label on the beer bottle
- `Durban's Famous Mutton Bunny Chow.jpg` — Coca-Cola can in frame
- `Braai Parys, street vendor.jpg` — SPAR + Coca-Cola umbrella branding
- `Food stall1.jpg` — Coca-Cola fridge, menu text, Flora logo

Approved and deployed: braai-oom, jozi-hustler, slang-friends-talking, slang-braai,
sa-braai-scene, sa-food-drink, scenario-braai-day-match, plus two CC BY-SA/CC0
Commons photos (Bunny Chow with Lamb & Potato; Moving market in Soweto).

### Verification (the mandatory chain, not the DB)

1. `wp_update_post` returned OK — **not accepted as proof.**
2. **Re-fetched each live logged-out rendered page** with a cache-buster:
   221 → 2 figures, 301 → 2, 319 → 2, 327 → 2, 342 → 2. Distinct `src` per figure.
3. **Positioning check** caught a real defect: on first insert both figures landed at
   the *same* h2 depth (stacked, no text between). A reposition pass moved figure 1 to
   hero position and figure 2 to ~50–55% depth. Re-verified on the live page.
4. All 8 media URLs return HTTP 200.
5. Server-side re-read: **34/34 Articles have ≥2 in-body images, 0 missing featured
   images, minimum word count 1,566.**

Hero position sits after the byline / "Last updated" / Table of Contents block — this
matches the site's own established convention (verified against control post 199).

### Housekeeping
All 5 diagnostic/deploy scripts removed from the public web root via FTP; each
verified **404**. (Leaving a script in a web root is what caused a real outage
previously — see the audit skill's `curl -sI` pitfall.)

## Still open (portfolio-wide)

115 − 5 = **110 Articles still have no in-body image**, all on other sites:

| Site | Articles | Missing in-body image |
|---|---|---|
| zadocs.co.za | 73 | 61 |
| whippetqr.com | 44 | 29 |
| 5minutes.co.za | 36 | 21 |
| howzitza.co.za | 34 | **0 ✅ closed this cycle** |
| beanel / sumza / saymyname | 113 | 0 |

Secondary: zadocs serves 73 Articles from only 13 distinct featured images — same
shared-skeleton defect that drives the "low value content" rejections.

This is a content-generation pass (new, distinct, subject-checked images per Article),
not a copy pass. Filename-based reuse was tested and rejected earlier — it matched only
1 of 61 zadocs Articles meaningfully.
