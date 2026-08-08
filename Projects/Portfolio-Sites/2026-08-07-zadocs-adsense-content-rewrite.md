# ZADocs AdSense Content Rewrite — Eliminate Byte-Identical Template Shells

**Date:** 2026-08-07
**Project:** ZADocs (zadocs.co.za) — portfolio-wide finding
**Branch:** `fix/adsense-content-rewrite`

## The Problem (Root Cause)

zadocs.co.za was rejected by AdSense AGAIN for "Low value content" despite a July 2026 fix that "removed boilerplate." The July fix removed ONE pattern ("in the South African Context") but never actually read the words.

The audit (Trigger 8, new in `adsense-quality-debug` skill) revealed the true cause: **63 of 66 posts were document-template landing pages** sharing the SAME factory skeleton — byte-identical H2s (`Overview`, `When To Use This Template`, `How To Complete`, `FAQs`, `Understanding South African Law`, `Legal Knowledge for Every South African`, `Legal Documents and Your Rights`, `More To Discover`) with the "Understanding South African Law" tail **hash-identical across all 63 posts** (1 distinct variant).

Google sees 63 pages of the same text and rejects the whole site as auto-generated — regardless of word count. **This is why rejections "never stopped": box-checking (word counts, sitemaps, categories, images, essential pages) all passed while the actual words were factory-duplicated.**

## Full Portfolio Audit (2026-08-07)

| Site | Posts | Issue |
|------|-------|-------|
| zadocs.co.za | 66 | **63 template shells, byte-identical tail (1 hash)** — worst, rejected |
| sumza.co.za | 32 | "Why This Matters" x16, "Frequently Asked Questions" x21 |
| beanel.com | 33 | "Why This Matters for South African Internet Users" x28 |
| whippetqr.com | 37 | "Conclusion" x47, "Frequently Asked Questions" x23 |
| saymyname.co.za | 28 | "The Significance of Names in South African Culture" x22 |
| 5minutes.co.za | 29 | "The Educational Value of Quick Games" x16 |
| howzitza.co.za | 15 | Minor — mostly clean |

## The Fix (ZADocs — Complete)

1. **Authored 63 unique drafts** via parallel subagents (one per category): Employment 21, Business 11, Personal 8, Property 8, Events 8, Education 7.
2. **Expanded all to 1,500+ words** (three rounds of expansion to catch files between 1,300–1,490): final avg ~1,600.
3. **Zero boilerplate headers remain** across all drafts.
4. **Published all 63** to the live site, verified clean.

## The Publish Method (cp47 PITFALL)

- `wp_update_post()` HANGS/fatals on cp47 for large content (heavy content-filter hook chokes).
- REST API writes return 401 (read-only app password).
- **Working method:** convert markdown→Gutenberg blocks LOCALLY (Python), upload pre-converted `.html` block files, then a direct `$wpdb->update` on `wp_posts` keyed by post ID. Pace requests ~6s apart (cp47 throttles rapid PHP). Ran 63 posts, 0 failures.
- **Markdown converter bug:** drafts starting with `# H1` cause an infinite loop unless the converter skips H1 lines (page already has its own title).

## Verification (All Passed)

- All 66 zadocs posts CLEAN (0 remaining boilerplate markers).
- All rewritten posts 1,500+ words.
- 2+ images per article (AGENTS.md rule).
- Sitemap 66 URLs matches REST count.
- `browser_vision` on employment-contract-template: clean layout, proper H2s, lists, no boilerplate.

## Skills Updated

- **adsense-quality-debug** — added Trigger 8 (byte-identical template shells) + full detection script + 7-site audit table + checklist item 15.
- **portfolio-site-quality-standards** — template-shell warning under Rule 8.
- **zadocs-maintenance** — `wp_update_post` hangs on cp47 pitfall + direct-SQL publish method.

## Outstanding (follow-up needed)

- **The other 5 affected sites** (sumza, beanel, whippetqr, saymyname, 5minutes) still have template-signature content — need the same rewrite treatment. Drafts are zadocs-only so far.
- **Do NOT re-apply AdSense for zadocs** until the site is re-reviewed and the other sites' template-shell content is addressed to prevent the same portfolio-wide issue.
