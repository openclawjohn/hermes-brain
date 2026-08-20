# SayMyName — Sepedi Article Image Fix (2026-08-20)

## Issue Detected
The weekly cron created a new article on saymyname.co.za:
- **Title:** Sepedi Baby Names: Meanings and Cultural Significance
- **URL:** https://saymyname.co.za/sepedi-baby-names-meanings-and-cultural-significance/
- **Post ID:** 312

The article was published with **ZERO images** (featured_media=0, no `<img>`/`<figure>` tags in content) — violating the 2-image quality standard.

## Root Cause
The saymyname theme ignores `featured_media` (known pitfall). Existing posts embed BOTH images as `<figure>` blocks directly in `post_content` (hero at top + in-content mid-article). The new post was created without any embedded images.

## Fix Applied
1. Sourced 2 real Wikimedia Commons CC photos (both inspected via vision_analyze — no text/logos/watermarks):
   - **Featured (hero):** `File:Sepedi culture.jpg` → uploaded as media 313 (`sepedi_culture.jpg`) — 4 women in traditional Sepedi attire
   - **In-content:** `File:Zulu and Sepedi traditional attires.jpg` → uploaded as media 314 (`sepedi_attires.jpg`) — 2 women in Sepedi/Zulu attire
2. Embedded hero `<figure>` at top of `post_content` (0% position)
3. Embedded in-content `<figure>` at ~55% through content (before H2 index 5 of 10)
4. Set `featured_media` = 313 (for schema/SEO)
5. Used `max-width:100%;width:auto` inline style (NOT `width:100%`) to prevent stretching

## Verification (rendered page, logged-out)
- HTTP 200 ✅
- 2 `<img>` tags ✅
- Fig0: sepedi_culture.jpg (h2_before=0/10, 0%) — hero ✅
- Fig1: sepedi_attires.jpg (h2_before=5/10, 50%) — in-content ✅
- Both images DIFFERENT subjects ✅

## Site Health (saymyname.co.za)
- Post count: 30 (was 29 — new article)
- Essential pages: /about/ 200, /contact/ 200, /privacy-policy/ 200, /terms-of-service/ 200, /contact-us/ 200
- ads.txt: 200 · AdSense meta: count 1 · sitemap_index.xml: 200

## Lesson
When the weekly cron creates articles, it must embed BOTH images as `<figure>` blocks in `post_content` (not rely on `featured_media`), because the saymyname theme ignores `featured_media`. This is the same pattern as existing posts.
