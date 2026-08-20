# SayMyName "Low Value Content" Fix — 2026-08-19

## Trigger
Google AdSense flagged saymyname.co.za for **"Low value content"** (Aug 11, 2026 notice, surfaced Aug 19).

## Root Cause (decisive, byte-verified)
The site was previously marked "Clean" in PROJECT_STATE (2026-08-18), but that audit hashed **raw HTML** (where image tags/whitespace differ), missing that the **stripped plain text** of 4 sections was a copy-paste factory template. The correct test — stripping tags, normalizing whitespace, then byte-hashing each H2's section text across all posts — proved **3 sections byte-identical across many articles**:

| Boilerplate section | Words | Posts using byte-identical copy |
|---|---|---|
| "The Significance of Names in South African Culture" | 654 | **17** |
| "The Importance of Names in South African Culture" | 238 | **14** |
| "More About African Naming Traditions" | 228 | **13** |
| "Choosing a Business Name in South Africa" | 257 | **5** |

20 of 29 articles carried these blocks. Stripped of filler, most dropped to **575–1,100 words** — they only "hit" 1,500 by reusing boilerplate. This is Google's exact "Low value content" signature.

## What Was Done
- **Authored unique top-up sections** for all 20 affected articles (via parallel subagents, one per slug) — genuinely original, topic-specific prose, no forbidden headings.
- **Removed all byte-identical boilerplate blocks** from each affected article's `post_content`.
- **Spliced in the unique top-ups** → every article now 1,500+ words.
- **Deployed** via the proven cp47 direct `$wpdb->update` PHP method (REST writes are read-only, `wp_update_post` hangs on cp47).
- **Purged LiteSpeed cache** at `/home/whippetq/lscache`.
- **Cleaned up** all temp PHP scripts and upload dirs from the server.

## Verification (all live from server)
- ✅ **0 boilerplate H2 headings** remaining across all 29 posts
- ✅ **0 byte-identical sections** remaining (re-ran the decisive cross-post hash test)
- ✅ **All 20 affected posts ≥1,500 words** (range 1,508–2,133)
- ✅ Rendered pages HTTP 200, no boilerplate in rendered HTML

## Remaining (images)
13 posts render only 1 image (some hero images are also topic-irrelevant, e.g. Joburg CBD on a Zulu-baby-name article). This is a **separate** quality task from the Low-Value-Content rejection — flagged for a follow-up pass.

## Files
- Branch: `fix/saymyname-boilerplate-content`
- Commit: artifacts in `docs/saymyname-adsense-fix-2026-08-19/`
- Obsidian: `Projects/Portfolio-Sites/2026-08-19-saymyname-low-value-content-fix.md`
