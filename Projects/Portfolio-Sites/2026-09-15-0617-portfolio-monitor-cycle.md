# Portfolio Monitor Cycle — 2026-09-15 06:17 SAST

## Verdict
No server status change. All 7 sites **Phase F maintenance (stable)** — homepages 200 across 3 passes,
zero oscillation, sitemap↔REST parity exact 1:1 on all 7, essentials 200, ads.txt 200 ×7, AdSense meta 1 ×7,
0 real `-2`/`-3` slugs. Content counts identical to the 06:11 cycle.

**The AdSense blocker is now precisely quantified — and it is NOT what the previous two cycles reported.**

## Headline correction (important)

The previous cycle (06:00) reported *"87 zero-image / 45 one-image Articles"* and *"13 Articles have no featured image"*.
Both were **artifacts of the external curl sweep being LiteSpeed-throttled**, not site state. Two independent proofs:

1. `zadocs.co.za/how-to-draft-a-rental-agreement-in-south-africa/` was counted as "0 images" in that sweep —
   it renders **2** `<figure>` images with cache-buster and spacing.
2. On direct re-test, pages known to have 2 images (beanel, zadocs) returned `0 figs | 0 imgs` on a burst request
   and correct counts a minute later. A burst returns **partial HTML with HTTP 200 and full `size_download`** —
   a silent truncation, which is exactly what corrupts an image count derived from `rendered` HTML.

**Rule established:** never derive an image count from externally-fetched `content.rendered` on this server.
Counts taken that way are unreliable in both directions. Server-side PHP inventory is authoritative.

## Authoritative server-side inventory (PHP via FTP, no external throttle)

| Site | Articles | content image | no content image | featured thumb assigned (all) |
|------|---------|---------------|------------------|-------------------------------|
| beanel.com | 40 | — | — | — |
| howzitza.co.za | 34 | 30 | **4** | 4/4 |
| sumza.co.za | 39 | 39 | 0 | ✅ |
| zadocs.co.za | 73 | 12 | **61** | 61/61 |
| saymyname.co.za | 34 | 34 | 0 | ✅ |
| whippetqr.com | 44 | 15 | **29** | 29/29 |
| 5minutes.co.za | 36 | 15 | **21** | 21/21 |

**115 Articles have no image inside the article body.** All 115 already have a featured image assigned in the DB.

## Root cause (established, not inferred)

- `_thumbnail_id` is set on every one of the 115 (`thumb=0` count is zero portfolio-wide).
- The theme renders it — verified in live HTML: zadocs `Johannesburg_skyline-scaled.jpg`,
  whippetqr `how-to-add-a-logo-to-your-qr-code…jpg`, 5minutes `jhb-dusk.jpg`,
  howzitza `on_Mandela_Square…Johannesburg-scaled.jpg` all present in page output.
- The gap is purely the **in-content image**: `post_content` contains `figure=0 img=0 wp:image=0`.
  Every one was written by a content pass that never inserted an in-content image.

**Secondary finding — duplicate featured images:** zadocs serves 73 Articles from only **13 distinct**
featured images (759→21, 760→20, 758→20). Same pattern on sumza (10 distinct for 39) and saymyname (32 for 34).
The 61 zadocs gaps and the 13-image pool are the *same defect*: zadocs content is template-farmed from a
single shared skeleton, which is the documented driver of AdSense "low value content" rejection.

## Why the fix was NOT applied this cycle

Auto-assignment from the existing media library was evaluated and **rejected on evidence**: filename matching
is wrong at scale. `5minutes-fun-baby-shower-games…` matched 5 unrelated 5minutes Articles; only 1/61 zadocs
Articles matched anything meaningful. Assigning those would have injected wrong-topic images and *increased*
duplication — directly against the user's repeated rejection of "same concept repeated across articles".

Correct fix requires new topical images (one distinct, subject-checked image per Article) written into
`post_content` at ~55% depth. That is a content-generation operation, not a copy operation, and must not be
faked by reuse. **Not done — needs a dedicated cycle.**

## Fixed this cycle

- Removed 4 diagnostic PHP scripts from public web roots after use (verified 404):
  `hermes_diag_images.php`, `hermes_diag_theme.php`, `hermes_inv.php` on zadocs / 5minutes / howzitza / whippetqr / sumza / saymyname.
- Recorded the two false-alarm mechanisms in the audit skill so no future cycle repeats them.

## Verified server-side

- zadocs: Astra Child v4.13.4, 33 media attachments, no `astra_post_thumbnail` filter override, no `the_post_thumbnail` filter.
- whippetqr: featured renders correctly (its `entry-content` has no figures because the theme puts the featured image outside the content div).
- Essential-page 301s (`/terms/`→`/terms-of-service/`, `/about/`→`/about-us/`, `/about-us/`→`/about/`, `/terms-of-use/`) all resolve **200** at final destination — canonical redirects, not faults.

## Remaining tasks (priority order)

1. **115 Articles with no in-content image** — zadocs 61, whippetqr 29, 5minutes 21, howzitza 4. Needs new distinct topical images; PHP write pass via FTP.
2. **Duplicate featured images** — zadocs 13 images for 73 Articles; sumza 10 for 39. Same shared-skeleton defect behind the AdSense rejection history.
3. howzitza `/terms/` and `/terms-of-service/` both resolve — confirm canonical only (minor).
4. Pre-existing content backlog: 18 sumza Articles sharing a stale FAQ block; ~8 whippetqr Articles with duplicated sentences; 3 5minutes Articles with identical CTA.
