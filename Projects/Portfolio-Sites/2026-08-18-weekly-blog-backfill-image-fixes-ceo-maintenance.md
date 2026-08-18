# 2026-08-18 — Weekly Blog Backfill + Image Fixes + CEO Backfill + Maintenance

## Context
Machine was off **Mon Aug 10 ~22:22 → Sat Aug 15 15:02**. Crons scheduled in that window missed their runs. Daily/continuous crons self-resumed on Aug 16; the one genuine miss was **Weekly Blog Posts** (Tue Aug 11 02:00).

## What was done

### 1. Weekly Blog Posts backfill (7 articles, 1/site) — VERIFIED
Triggered the cron manually. All 7 articles live (HTTP 200, logged-out), 2 distinct in-content images each, correctly positioned, real categories, in sitemaps:
- beanel.com — What Is a Firewall and Why Your Home Network Needs One
- howzitza.co.za — South African Wedding Traditions: From Umbondo to the Veil
- sumza.co.za — How to Calculate Your Fuel Consumption and Cost Per Kilometre in SA
- zadocs.co.za — How to Write a Letter of Demand in South Africa
- saymyname.co.za — Beautiful Afrikaans Baby Names and Their Meanings
- whippetqr.com — How to Create a WhatsApp QR Code for Your South African Business
- 5minutes.co.za — Fun South African Riddles and Brain Teasers

### 2. Image quality audit + 3 replacements (CRITICAL)
Ran mandatory `vision_analyze` pixel inspection on all 9 images (cron env lacked vision tools). **3 failed**:
- sumza fuel hero (`sumza-fuel-hero.jpg`) — "ORINO" brand text, "R" logo, numbers
- sumza fuel content (`sumza-fuel-content.jpg`) — receipt text (Cyrillic), screen numbers
- whippetqr WhatsApp content (`whippetqr-whatsapp-content.jpg`) — Chinese chars, logos

**Replaced all 3** with clean fal.ai Klein images (strict anti-gibberish prompts), inspected clean, uploaded via FTP, content updated via PHP, cache purged, verified live:
- sumza: `sumza-fuel-hero-2.jpg` (featured, attachment 793) + `sumza-fuel-content-3.jpg`
- whippetqr: `whippetqr-whatsapp-content-2.jpg`

### 3. CEO of Domains backfill (partial)
- **Quora (Wed):** 1 real answer posted + verified — bond calculator accuracy question, linked `https://sumza.co.za/bond-calculator/`
- **Pinterest (Thu):** NOT completed — save-from-URL fetches image but selection UI never renders; bridge lacks CDP file-upload
- **Reddit (Tue) / Medium (Fri):** BLOCKED — logged out in Chrome profile

### 4. Phase 3 maintenance checks (all 7 sites)
- ✅ All sites up (200)
- ✅ IndexNow keys (`.well-known/rankingo.txt`) all 200
- ✅ Sitemaps all 200; all 7 new articles present in post-sitemaps
- ✅ ads.txt all 200
- ✅ AdSense meta tag present on all 7
- ✅ No broken `-2`/`-3` slugs (whippetqr `create-qr-code-business-3-steps` = false positive, "3 steps")
- ✅ No missing image alt text on recent articles
- ⚠️ **5minutes post 138** was 1475 words (under 1500) → **expanded to 1655 words**

### 5. 5minutes post 138 expansion (Elementor pitfall)
Post 138 is an **Elementor page** (`_elementor_edit_mode: builder`). `wp_update_post` updated DB but NOT the rendered content (comes from `_elementor_data`). Fixed via **Elementor MCP** `elementor_mcp_update_widget` on widget `7eb369a` (appended "Make the Most of Your Five Minutes" section). Verified live: 1655 words, new section renders.

### 6. LiteSpeed cache purge — KEY LEARNING
`\LiteSpeed\Purge::purge_all()` + `do_action('litespeed_purge_all')` did NOT clear the server page cache. The actual cache lives at **`/home/whippetq/lscache`** (NOT `wp-content/litespeed`). Must delete files there via PHP `rrmdir()` to force fresh render. This is why the 5minutes page stayed stale until I purged `/home/whippetq/lscache`.

## Not done / blocked
- **Pinterest pins** — not created (UI won't render selection; needs CDP file-upload or manual)
- **Reddit / Medium backfill** — blocked on user login
- **Obsidian doc / PROJECT_STATE / git** — this doc is the Obsidian piece; PROJECT_STATE + git pending

## Key files
- Credentials: `/home/m/credentials.json`
- Obsidian vault: `/home/m/Documents/HermesBrain/`
