# Weekly Articles — 2026-09-15 (Tuesday 02:00 cron)

## Summary
Published 1 new article on each of the 7 portfolio domains. All verified HTTP 200, 2 distinct
Wikimedia Commons photos each (featured top + in-content at ~42–46%), 2,168–2,487 words of body
text, real categories, present in sitemaps.

## Articles published

| Site | Article | Words | URL |
|------|---------|-------|-----|
| beanel.com | Why Is My Home WiFi Slow? 12 Causes and How to Fix Them | 2,168 | https://beanel.com/why-is-my-home-wifi-slow-12-causes-and-how-to-fix-them/ |
| howzitza.co.za | Namaqualand in Bloom: A Guide to South Africa's Spring Flower Season | 2,269 | https://howzitza.co.za/namaqualand-in-bloom-a-guide-to-south-africas-spring-flower-season/ |
| sumza.co.za | How to Calculate Overtime Pay in South Africa: A Complete Guide | 2,191 | https://sumza.co.za/how-to-calculate-overtime-pay-in-south-africa-a-complete-guide/ |
| zadocs.co.za | Antenuptial Contract in South Africa: How to Get Married Out of Community of Property | 2,248 | https://zadocs.co.za/antenuptial-contract-in-south-africa-how-to-get-married-out-of-community-of-property/ |
| saymyname.co.za | Venda Baby Names and Their Meanings: A Guide to Tshivenda Names | 2,191 | https://saymyname.co.za/venda-baby-names-and-their-meanings-a-guide-to-tshivenda-names/ |
| whippetqr.com | QR Codes for Doctors and Clinics in South Africa: A Practical Guide | 2,487 | https://whippetqr.com/2026/09/15/qr-codes-for-doctors-and-clinics-in-south-africa-a-practical-guide/ |
| 5minutes.co.za | Fun Baby Shower Games to Play in South Africa | 2,314 | https://5minutes.co.za/fun-baby-shower-games-to-play-in-south-africa/ |

All 7 topics are new — checked against the existing 39/33/38/72/33/43/35 posts per site, no repeats.

## Image gate — OCR text detection (new capability this session)

`vision_analyze` and browser tools are NOT available in the cron environment. Local ollama vision
models (`minicpm-v`, `moondream`, `llava`) time out on CPU — no GPU (`nvidia-smi` fails). Cloud
ollama models return HTTP 402 Payment Required.

**Solution:** installed `rapidocr-onnxruntime` (pure Python + onnx, no system deps, no sudo needed)
and wrote `/home/m/scripts/ocr_check.py`. This runs real OCR text detection over every candidate
image and rejects anything containing text, logos, watermarks, or brand names.

**This gate caught 4 defective images on the first pass**, all of which would otherwise have gone live:
- `qr_1` — a medication packet label (PAUTS, 1000mg, DIRECTIONS) — completely wrong subject
- `qr_2` — a clinic notice board ("CATCH IT, KILL IT", "PLEASE NOTE THAT IF YOU ARRIVE LATE")
- `5min_1` — "BABY GIRL" text on baby shower decorations
- `5min_2` — "LONDON, ENGLAND" text on a party image

All 4 were replaced with OCR-clean alternatives and re-verified. A 5th issue was caught by metadata
vetting: "Faces 1" (Shona sculpture from Zimbabwe) was rejected as the wrong culture for a Venda
article and replaced with "Venda Woman - Pottery" (CC BY-SA 4.0), OCR-clean.

Final: all 14 live images downloaded from the rendered pages and individually OCR-scanned — **ALL CLEAN**.

## Image sourcing
All 14 are real Wikimedia Commons photographs (the skill's preferred source) — no AI generation, so
no gibberish-text risk. Licences CC BY, CC BY-SA, or CC0. Metadata pre-filtered against a bad-word
list (logo, poster, sign, map, document, menu, certificate…) before download.

## Post-cron audit gate results

| Check | Result |
|-------|--------|
| Homepage 200 | 7/7 ✅ |
| Article 200 | 7/7 ✅ |
| 2+ figures, distinct srcs | 7/7 (2 each, all distinct) ✅ |
| In-content image position | 42–46% through, first figure at h2_before=0 ✅ |
| Word count ≥1,500 | 2,168–2,487 ✅ |
| Duplicate H2s | none ✅ |
| Real category (not Uncategorized) | 7/7 ✅ |
| Sitemap includes new article | 7/7 ✅, REST↔sitemap 1:1 parity |
| ads.txt | 200, correct pub ID ✅ |
| AdSense meta tag | count=1 on all 7 ✅ |
| Broken (-2/-3) slugs | 0 on all 7 ✅ |
| Nav says "Articles" | 7/7 ✅ |
| Essential pages | all reachable (some 301 to canonical variants) ✅ |
| New posts share a featured image | 0 — all unique ✅ |
| New articles contain site boilerplate | none ✅ |

## Fixed during the run
1. **howzitza duplicate article.** The first script run created ID 574 at slug `...flower-season-2`
   alongside ID 571 at the correct slug. The duplicate was deleted and 571 preserved.
   This was caused by re-running the deploy script to capture truncated output — the script's
   `get_page_by_path('{slug}')` guard missed because the requested slug had already been suffixed.
2. **All 7 sitemaps were stale** (missing the new article, LiteSpeed cache). Regenerated static
   `post-sitemap.xml`, `page-sitemap.xml`, `sitemap_index.xml` via PHP `file_put_contents()`.
   Re-verified: new article present in all 7 with exact REST parity.
3. **Server cleanup.** All 7 `weekly_deploy_0915.php`, `weekly_sitemap_0915.php`, `hz_cleanup.php`
   and the `__wk/` image directories removed from every server.

## Pre-existing issues found (NOT introduced this week — flagging for the backlog)
Per the post-cron-audit-gate low-value-content checks, the following pre-existing templated content
was detected. None of it is in this week's articles, but it is AdSense "low value content" exposure:

- **whippetqr.com — 50 repeated sentences across posts.** Identical FAQ blocks repeated 3× verbatim:
  "how do qr codes work in south africa?", "can i create a qr code for free?", "what is the best size
  for a qr code in print?", "why won't my qr code scan?" (8 duplicated Q blocks).
- **sumza.co.za — 14 repeated sentences**, each appearing **18×** across posts, e.g. "Whether you are
  saving for retirement, investing in property, or reducing your electricity costs, taking action
  today is almost always better…" and "South Africa has unique regulations, market conditions, and
  consumer protections that differ from other countries."
- **5minutes.co.za — templated CTA** "For more South African games and activities, visit
  5minutes.co.za." repeated verbatim on 3 posts, plus 3 duplicated question blocks.
- **Duplicate featured images (pre-existing):**
  - zadocs: media 758/759/760 each used on 20–21 posts
  - 5minutes: media 286–291 each used on 3–5 posts
  - whippetqr: media 293 used on 2 posts
- **sumza: media 457/459/531 each used on 7–12 posts.**
- beanel.com, howzitza.co.za, saymyname.co.za are clean on all four counts.

Recommended next action: a dedicated de-templating pass — remove the duplicated FAQ/CTA blocks from
all but one post and re-expand each shortened post with original article-specific content to stay
above 1,500 words, plus redistribute the shared featured images.

## Method notes
- Content created via PHP scripts uploaded by FTP (REST API throttles) — proven pattern.
- beanel.com is on 164.160.91.56 with separate FTP creds; the other 6 are on cp47.
- No subagent/`delegate_task` tool exists in this cron environment, so the audit gate was executed
  directly with the same rigour the gate prescribes (the gate's own rule notes subagents fail at
  bulk content work 14/14 times; they are reserved for audits).
