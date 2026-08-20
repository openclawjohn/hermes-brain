# 2026-08-20 Auditor Second Course — Deeper Policy + UX Checks

## Date
2026-08-20

## What was done

### 1. Auditor "second course" of knowledge enrichment
Sent the auditor back to "internet university" — re-fetched Google's authoritative Publisher Policies + "site ready for AdSense" checklist. It came back smarter and found **real issues the first audit missed**.

### 2. 10 new checks added to `~/.hermes/scripts/adsense-auditor.py`
| Check | Google policy / rationale |
|-------|---------------------------|
| `privacy_disclosure` | Privacy disclosures policy — must disclose third-party cookies/beacons/IP from ad serving |
| `language` | Unsupported languages policy — `<html lang>` present |
| `viewport` | Site-readiness UX — mobile-friendly |
| `title_meta` | Unique title + meta description per page |
| `h1` | Exactly one H1 per article |
| `image_alt` | Accessibility — images have alt text |
| `internal_links` | Clear navigation / UX — article links to other pages on same site |
| `canonical_self` | Indexing — canonical points to the page itself (past real failure) |
| `sitemap_https` | No mixed content |
| `duplicate_titles` | Unique content — no two posts share a title |

### 3. Real issues found + fixed
**whippetqr duplicate titles (FAIL):** 2 posts shared "QR Code Marketing: Creative Campaign Ideas for South African Businesses" (IDs 282 + 197, different content). Renamed ID 282 to "QR Code Marketing Campaign Ideas: Creative Examples for South African Brands" + new slug.

**Internal links missing on 6 of 7 sites (WARN):** recent articles had ZERO internal links in their bodies (sumza was the only one passing). Added a "Related Reading" block with 3 contextual links to each flagged article:
- whippetqr (5), howzitza (5), zadocs (5), saymyname (5), 5minutes (5), beanel (5) = **30 articles total**

### 4. Final audit — all 7 sites pass with 0 FAILs
| Site | FAILs | WARNs | Ready? |
|------|:-----:|:-----:|:------:|
| beanel.com | 0 | 0 | ✅ |
| howzitza.co.za | 0 | 2 | ✅ |
| sumza.co.za | 0 | 1 | ✅ |
| zadocs.co.za | 0 | 1 | ✅ |
| saymyname.co.za | 0 | 2 | ✅ |
| whippetqr.com | 0 | 1 | ✅ |
| 5minutes.co.za | 0 | 1 | ✅ |

Remaining WARNs are non-blocking quality items: missing meta descriptions (howzitza, sumza), missing image alt (howzitza, saymyname), boilerplate markers that are natural-prose false positives (zadocs, saymyname, whippetqr, 5minutes).

## Key decisions
- The auditor keeps getting smarter with each enrichment course. First course caught nav/Articles issues; second caught duplicate titles + missing internal links.
- Run the auditor after ANY content change — it now checks 30+ gates including deep UX/policy items.

## Files changed
- `~/.hermes/scripts/adsense-auditor.py` — added 10 new policy/UX checks
- `~/.hermes/skills/wordpress/adsense-quality-debug/SKILL.md` — added "SECOND COURSE" section
- Live sites: whippetqr (dup title fix + 5 internal links), howzitza/zadocs/saymyname/5minutes/beanel (5 internal links each)

## Verification
- Auditor re-run (report-13.json): all 7 sites 0 FAILs
- Internal links verified rendering on all 6 fixed sites (3 links each)
- Duplicate titles verified gone on whippetqr
