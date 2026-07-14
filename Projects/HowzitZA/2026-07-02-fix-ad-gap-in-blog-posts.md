---
created: 2026-07-02
tags: [howzitza, fix, adsense, layout]
---

# Fix Ad Gap in Blog Posts

## Problem
The slang guide blog post (`/2026/06/29/the-ultimate-guide-to-south-african-slang-100-words-and-phrases-every-mzansi-local-knows/`) had a huge white gap between the intro paragraph and the "Everyday Greetings & Expressions" heading. An unfilled AdSense iframe (280px) was sitting right after the first `</p>` tag.

## Root Cause
`howzitza-game-engine.php` line 796-821 — the `howzitza_ad_slots()` function injected an ad after the **first** `</p>` tag in the post content. For this blog post, the first paragraph is the intro, so the ad appeared between the intro and the first heading, creating a massive empty gap when the ad didn't fill.

## Fix
Changed the ad insertion logic for `is_singular('post')` from "after 1st paragraph" to "after 3rd paragraph":
- Loops through 3 `</p>` occurrences before inserting the ad
- Falls back gracefully if fewer than 3 paragraphs exist
- Homepage ad placement unchanged (still after 1st paragraph)

## Files Changed
- `wp-content/mu-plugins/howzitza-game-engine.php` — 9 insertions, 4 deletions

## Verification
- Visual check confirmed: intro paragraph now flows directly into "Everyday Greetings & Expressions" heading with consistent spacing
- Ad now appears after the 3rd slang entry (after "Howzit", "Sharp sharp!", "Lekker")
- Git commit: `fdd08d2`
