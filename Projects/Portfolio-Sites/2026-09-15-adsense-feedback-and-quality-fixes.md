# 2026-09-15 — AdSense Feedback Read Live + Quality Defect Fixes

**Task:** Check up on all websites + Google AdSense feedback, fix what's necessary.
**AdSense/Google access method:** Chrome Connector bridge (port 16319) against the user's logged-in Chrome. No guessing.

---

## 1. AdSense feedback — read directly from the dashboard

Source: `adsense.google.com/adsense/u/0/pub-1162021827795507/sites/list` (read 2026-09-15 ~18:50 SAST)

| Site | Approval status | Status details | ads.txt status | Last updated |
|---|---|---|---|---|
| **howzitza.co.za** | **Needs attention** | **Low value content** | Authorized | Sep 12, 2026 6:10 AM SAST |
| beanel.com | Getting ready | — | Not found | Sep 7, 2026 3:19 PM SAST |
| sumza.co.za | Getting ready | — | Not found | Jul 21, 2026 1:36 PM SAST |
| zadocs.co.za | Getting ready | — | Not found | Aug 8, 2026 4:59 PM SAST |
| saymyname.co.za | Getting ready | — | Not found | Sep 7, 2026 3:20 PM SAST |
| 5minutes.co.za | Getting ready | — | Not found | Sep 7, 2026 3:20 PM SAST |
| whippetqr.com | Getting ready | — | Authorized | Jul 15, 2026 10:30 AM SAST |

**Conclusion: only ONE site has an actual policy violation — howzitza.co.za, "Low value content", flagged Sep 12, 2026.**
The other six are in the normal queue with no violation. "Request review" was deliberately NOT submitted for howzitza — the button exists on `sites/detail/url=howzitza.co.za`, but submitting before the underlying content defect is cleared would risk a second rejection on the same grounds.

### ads.txt "Not found" — diagnosed, NOT a file problem
All 7 serve an identical 59-byte ads.txt with the correct pub ID and trailing newline:

- `beanel.com` 200 / `howzitza.co.za` 200 / `sumza.co.za` 200 / `zadocs.co.za` 200 / `saymyname.co.za` 200 / `whippetqr.com` 200 / `5minutes.co.za` 200
- Verified serving 200 to `AdsBot-Google`, `Googlebot/2.1`, `Mediapartners-Google` and the Android Googlebot UA
- Identical `content-length: 59`, `content-type: text/plain`

howzitza and whippetqr show **Authorized** with byte-identical files; the other five show **Not found**. Therefore the difference cannot be file content, headers, content-type, trailing newline or user-agent handling. It is Google-side crawl recognition. **Do NOT re-upload with a new timestamp** — that has been tried across multiple sessions and is not a new fix (per the adsense-quality-debug "3+ sessions" hard rule).

### Search Console — all 7 verified, all sitemaps Success
Properties confirmed present on the property selector: 5minutes, beanel, howzitza, saymyname, sumza, whippetqr, zadocs — all "Domain property".

| Site | Sitemap | Status | Last read | Discovered pages |
|---|---|---|---|---|
| howzitza.co.za | sitemap_index.xml | Success | Sep 14, 2026 | 33 |
| sumza.co.za | wp-sitemap.xml | Success | Sep 12, 2026 | 106 |
| sumza.co.za | sitemap_index.xml | Success | Sep 10, 2026 | 39 |
| zadocs.co.za | sitemap_index.xml | Success | Sep 13, 2026 | 72 |
| beanel.com | sitemap_index.xml | Success | Sep 12, 2026 | 39 |
| saymyname.co.za | sitemap_index.xml | Success | Sep 12, 2026 | 33 |
| 5minutes.co.za | sitemap_index.xml | Success | Sep 10, 2026 | 36 |
| whippetqr.com | sitemap_index.xml | Success | Jul 30, 2026 | 62 |
| whippetqr.com | wp-sitemap.xml | Success | Sep 12, 2026 | 44 |

Note: **sumza and whippetqr each have TWO sitemaps submitted** (both a native `wp-sitemap.xml` and a Rank Math `sitemap_index.xml`). Duplicate sitemap submission is untidy but both are Success. `sumza/robots.txt` points at `wp-sitemap.xml` while its Rank Math sitemap is also live — worth consolidating in a later pass.

### howzitza Search Console indexing report
"Not found (404)" 13 · "Page with redirect" 4 · "Excluded by noindex" 3 · "Soft 404" 1 · "Alternate page with proper canonical tag" 1 · "Crawled – currently not indexed" 8 (Google systems) · "Duplicate, Google chose different canonical than user" 1 · "Discovered – currently not indexed" 1.
The 404 list includes old date-based permalinks (`/2026/06/20/...`) and `/blog/` — legacy URLs from a permalink change, plus `/sample-page/`. No action taken; these are historical.

---

## 2. Independent auditor run (full)

```
python3 ~/.hermes/scripts/adsense-auditor.py --json /home/m/adsense-audit-report.json
```

| Site | FAILS | WARNS | Ready |
|---|---|---|---|
| beanel.com | 0 | 7 | ✅ |
| howzitza.co.za | 1 | 8 | ❌ |
| sumza.co.za | 0 | 6 | ✅ |
| zadocs.co.za | 1 | 8 | ❌ |
| saymyname.co.za | 0 | 10 | ✅ |
| whippetqr.com | 1 | 10 | ❌ |
| 5minutes.co.za | 2 | 6 | ❌ |

FAILs found:
- howzitza — duplicate_titles (1)
- zadocs — images (1 post <2 images)
- whippetqr — images (2 posts <2 images)
- 5minutes — template_shells + replication (byte-identical sections)

---

## 3. Fixes deployed

### 3a. Broken empty-src images (renders as a broken image icon to visitors)
A `<figure>` block whose `<img src="">` was empty. Removed the whole figure.

| Site | Post | real imgs | empty fixed |
|---|---|---|---|
| howzitza.co.za | 553 south-african-street-food-flavourful-journey-mzansi | 2 | 2 |
| sumza.co.za | 747 how-to-calculate-your-savings-goal-in-south-africa | 2 | 1 |
| sumza.co.za | 742 retirement-savings-south-africa-guide | 2 | 1 |
| zadocs.co.za | 1178 how-to-write-a-last-will-and-testament-in-south-africa | 1 | 2 |
| beanel.com | 1000 what-is-a-mac-address-and-why-it-matters | — | 2 |
| saymyname.co.za | none | — | 0 |
| 5minutes.co.za | none (images live in Elementor data, not post_content) | — | 0 |

Method: `fix-empty-img.php` uploaded via FTP, run dry first, then written, then deleted (404 verified).
Note: this fixer walks `post_content` only — on 5minutes images live in `_elementor_data`, hence 0 matches there; that is expected, not a failure.

### 3b. howzitza duplicate + near-duplicate titles (the actual "low value content" driver)
howzitza had 3 near-duplicate topic pairs and 12 posts all published on 2026-08-20. Renamed 4 titles so no two articles share a title:

| ID | Old title | New title |
|---|---|---|
| 457 | The 11 Official Languages of South Africa: A Complete Guide | South Africa's 11 Official Languages: Origins and History |
| 327 | South African Street Food: From Kota to Bunny Chow | Kota, Bunny Chow and Braai Rolls: Street Food Classics |
| 301 | South African Slang: The Ultimate Guide to Mzansi Lingo | Mzansi Slang: 100 Words and Phrases Locals Actually Use |
| 413 | South African Slang: A Complete Guide to Speaking Like a Local in Mzansi | How to Speak South African Slang Like a Local |

Result: **0 duplicate titles.** All new titles 45–57 chars (within the 20–70 target).

### 3c. Byte-identical replicated content

**5minutes.co.za** — 3 posts shared a byte-identical 442-word block, "The Educational Value of Quick Games" (hash 7b2fadf46c84):
IDs 457 (traditional-south-african-games-morabaraba-diketo), 207 (how-to-host-a-game-night-south-african-style), 206 (games-to-play-on-long-south-african-road-trips).
Replaced with unique, topic-specific prose (578w / 476w / 495w). Word counts after: 1957 / 1782 / 1758 — all above the 1,500 floor.

**whippetqr.com** — two replicated clusters:
- "Conclusion" identical (71w, hash 6d34456a6c) on IDs 246, 244, 238
- "Conclusion" identical (74w, hash 61cc376e58) AND "Frequently Asked Questions" identical (781w, hash 38bd7ce039) on IDs 240, 215, 210

Replaced all 9 blocks with unique prose. Word counts after: 1985 / 1785 / 1907 / 1671 / 1900 / 1751 — all above 1,500.

Method: `*-deblock.php` uploaded with a sidecar replacements dir, dry-run validated (including a word-floor check), written, then script + dirs deleted and 404-verified.

### 3d. Verification — replicated content re-check
Stripped-text full-section SHA1 hash across 5 sites × all posts:

```
5minutes.co.za    posts=36  byte-identical sections: 0
howzitza.co.za    posts=34  byte-identical sections: 0
zadocs.co.za      posts=73  byte-identical sections: 0
saymyname.co.za   posts=34  byte-identical sections: 0
whippetqr.com     posts=44  byte-identical sections: 0
```

---

## 4. Live rendered-page verification (all HTTP 200, all fixes confirmed)

| Site | Article | imgs | empty | words |
|---|---|---|---|---|
| howzitza | south-african-street-food-flavourful-journey-mzansi | 2 | 0 | 1926 |
| howzitza | south-african-languages-11-official | 2 | 0 | 1755 |
| howzitza | the-11-official-languages-of-south-africa-a-complete-guide | 2 | 0 | 2001 |
| sumza | how-to-calculate-your-savings-goal-in-south-africa | 2 | 0 | 2192 |
| sumza | retirement-savings-south-africa-guide | 2 | 0 | 1792 |
| 5minutes | traditional-south-african-games-morabaraba-diketo | 0* | 0 | 2199 |
| 5minutes | how-to-host-a-game-night-south-african-style | 0* | 0 | 2029 |
| 5minutes | games-to-play-on-long-south-african-road-trips | 0* | 0 | 2003 |
| whippetqr | how-to-create-vcard-qr-code-digital-business-card | 2 | 0 | 1843 |
| whippetqr | what-is-a-qr-code-beginners-guide-sa | 2 | 0 | 2090 |
| whippetqr | qr-code-vs-barcode-difference | 3 | 0 | 1925 |

\* 5minutes articles are Elementor-built; article images render outside the `post_content` block my extractor reads. **Not fixed — needs a separate Elementor-side check.**

---

## 5. Still open / NOT fixed

1. **zadocs.co.za — 62 of 73 articles contain ZERO images.** Verified on live pages: the only `<img>` tags are the theme logo (`cropped-Second-Logo.png`) twice. 71 of 73 posts DO have `featured_media` set, and the media library holds images — but the theme does not render `the_post_thumbnail()` and the images are not embedded in `post_content`. This is the portfolio's largest remaining quality gap and a genuine AdSense risk. **This is the next job.**
2. **howzitza — "Low value content" not yet cleared.** Titles are now unique, but the 3 near-duplicate topic pairs (languages / street food / slang) remain as overlapping subjects, and 12 posts share the 2026-08-20 publish date. No "Request review" submitted.
3. **whippetqr — 2 posts still report <2 images** to the auditor; **beanel post 900** has 1 image; **saymyname** 1 post.
4. **5minutes article images not verified** (Elementor storage).
5. Remaining WARNs on all sites (non-blocking): `internal_links` (4–5 sampled posts with none), `sitemap_lastmod` (static sitemaps omit `<lastmod>`), `image_formats` (no WebP/AVIF), `aria_labels`, `modified_dates_consistent`, `ttfb` (saymyname 3/3 >600ms).
6. **sumza + whippetqr have duplicate sitemaps submitted** in Search Console; sumza robots.txt points at `wp-sitemap.xml` while Rank Math `sitemap_index.xml` is also live.

---

## 6. Housekeeping completed
- All fixer PHP scripts deleted from all document roots and 404-verified: `fix-empty-img.php` (5 sites), `hz-titles.php`, `fm-deblock.php`, `wq-deblock.php`
- Sidecar replacement dirs removed: `/5minutes.co.za/fmrepl`, `/public_html/wqrepl`
- Chrome tab opened for AdSense/Search Console closed; only the user's own tab (`ollama.com/settings`) remains

---

## 7. Lessons for the skills
- **Read AdSense live before diagnosing.** The real verdict (one "Low value content" violation on howzitza) was materially different from what the auditor implied (4 sites with FAILs). AdSense status and auditor status measure different things.
- **An empty `src=""` inside a `<figure>` is a silent broken image.** Neither the auditor's `<2 images` check nor a raw HTML grep flags it reliably — count empty srcs explicitly.
- **`grep -c entry-content` on a rendered page is not a valid article-body extractor.** On sumza the class appears inside an inline `<style>` block, and the regex silently captured a CSS rule and reported `imgs=0 words=0`. Extract from `<h1>` to footer instead.
- **`str_word_count()` in PHP and `len(txt.split())` in Python disagree slightly** — budget ~5% headroom above the 1,500 floor when authoring replacements.
- **FTP `mkd` on a path whose parent doesn't exist gives 550**, and `nlst()` returns `.` and `..` which must be filtered before `rmd`.
- **whippetqr's document root is `/public_html`** (addon domain), not `/whippetqr.com` — the latter does not exist.
