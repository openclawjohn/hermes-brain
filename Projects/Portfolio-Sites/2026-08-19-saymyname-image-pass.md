# SayMyName Image Pass — 2 Distinct Real CC Images on All Articles

**Date:** 2026-08-19
**Site:** saymyname.co.za
**Branch:** `fix/saymyname-boilerplate-content`

## What & Why
AdSense flagged saymyname for "Low value content." The content fix (unique 1,500+ word articles, no byte-identical boilerplate) resolved the rejection. As part of "finish everything," I then completed the image-quality pass: every article must have **exactly 2 distinct, on-topic, real CC-licensed photos** (hero top + in-content ~55%), per the portfolio hard rule.

## What Was Done
1. **Sourced 23 articles' images** (29 total articles; 6 already had 2) from **Wikimedia Commons** via the API — the hard rule's required source. fal.ai generation was unreliable (blank outputs under load) and is against the rule anyway.
2. **Mandatory vision inspection** on every candidate with `vision_analyze` (reject text/logo/watermark/ad). Rejected & replaced:
   - Elephants (off-topic for Xhosa ceremony article)
   - "AMERICAN CULTURE" poster text in a Xhosa ceremony photo
   - Child's headband with printed serial numbers
   - "Door of Hope Children's Mission" sign photo
   - Wale Street shot with a QR-code sticker
3. **Uploaded via FTP** to `wp-content/uploads/saymaname-images/<slug>/`.
4. **Deployed via PHP** (`$wpdb->update`): set featured attachment + embedded content `<figure>`. **KEY: the saymyname theme ignores `featured_media`, so the hero image MUST be embedded in-content** (leading `<figure>` at top), matching how the swati/afrikaans reference articles already worked.
5. **Dedup fixed:** an earlier pass double-embedded content images (`content.jpg` + `content-1.jpg`). Removed duplicates, then re-embedded exactly one hero + one content per post.
6. **Purged LiteSpeed** cache (`/home/whippetq/lscache`).
7. **Verified rendered pages (logged-out curl):** all 29 articles show **2 distinct images, HTTP 200, 0 issues**.
8. **Visual QC** (browser screenshot): hero image renders correctly at top, no broken/blank images. (Noted a pre-existing unrelated nav issue: "Contact Us" appears twice in the header menu.)
9. **Deleted temp PHP scripts** (`imgdeploy`, `dedup`, `fiximg`, `purge`, `cipc_fix`) from the server.

## Key Learning
- **The theme does not render `featured_media` in the article body** — to guarantee the hero image displays, it must be embedded as a `<figure>` at the very top of `post_content`.
- Wikimedia Commons API rate-limits (HTTP 429) hard when hammered; use serial requests with exponential backoff, and never download the same filename twice (WordPress appends `-1`).

## Verification
- `curl -sL <slug>/` → 2 distinct `wp-content/uploads` image filenames, HTTP 200, on 100% of articles (0 issues).
- Browser screenshot visual QC of `popular-zulu-baby-names-meanings`: hero renders, layout correct.
