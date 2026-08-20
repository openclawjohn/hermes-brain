# 2026-08-20 Auditor "Internet University" Upgrade + CEO Dispatch Rules

## Date
2026-08-20

## What was done

### 1. Auditor "Internet University" education
Sent the independent AdSense auditor to research Google's authoritative requirements directly from the official help center (web_search/web_extract were broken — firecrawl not configured — so used curl):
- **Eligibility requirements** (answer/9724) — unique content, 18+, own the site
- **"Make sure your site's pages are ready for AdSense"** (answer/7299563) — the site-quality checklist: unique content, clear navigation, good UX
- **Google Publisher Policies** (answer/9335564) — content/behavioral/privacy rules
- **Inventory value policies** — "screens without publisher-content or with low-value content", "replicated content" (the exact policy behind template-shell rejections), "more ads than publisher-content"

### 2. Auditor upgraded with 5 new policy-based checks
Added to `~/.hermes/scripts/adsense-auditor.py`:
- `privacy_policy` — Google requires a privacy policy page, linked from homepage
- `contact_page` — a way for users to contact the publisher
- `navigation` — clear nav with an "Articles" link (per hard rule, not "Blog")
- `ad_density` — the "more ads than publisher-content" policy
- `replication` — the "replicated content" policy (byte-identical template shells)

### 3. Real issues the education caught — and fixed
The new `navigation` check flagged **3 sites with no "Articles" link**:
- **howzitza.co.za** — had "ZA Culture" → renamed to "Articles" (menu item 135)
- **zadocs.co.za** — no articles link → added "Articles" (menu item 1157)
- **whippetqr.com** — nav hardcoded in Elementor homepage HTML widget (post 15) → added "Articles" to both `_elementor_data` and `post_content`

All verified rendering on live pages.

### 4. Final upgraded audit — all 7 sites pass
| Site | FAILs | WARNs | Ready? |
|------|:-----:|:-----:|:------:|
| beanel.com | 0 | 0 | ✅ |
| howzitza.co.za | 0 | 0 | ✅ |
| sumza.co.za | 0 | 0 | ✅ |
| zadocs.co.za | 0 | 1 | ✅ |
| saymyname.co.za | 0 | 1 | ✅ |
| whippetqr.com | 0 | 1 | ✅ |
| 5minutes.co.za | 0 | 1 | ✅ |

Every new policy-based check passes on all 7 sites. Remaining WARNs are non-blocking: `boilerplate` on zadocs/saymyname/whippetqr/5minutes (verified natural prose, not byte-identical template shells — false positives).

### 5. CEO dispatch rules codified into skills
The CEO subagents failed completely on 2026-08-20 (all hit iteration caps, made zero changes). Codified the fix into two skills:
- `adsense-quality-debug` — "CEO DISPATCH RULES" section
- `ceo-of-domains` — "CEO DISPATCH RULES" section

**The 6 rules:**
1. ONE narrow task per dispatch — never a list
2. Pre-fetch content and hand it to the CEO in `context`
3. Give the exact fix method (one-shot PHP script pattern), not a mandate
4. Bound the scope to ~10–15 tool calls
5. Verify independently — never trust the self-report
6. Fall back to direct work when delegation fails

## Key decisions
- The CEO subagents are not incapable — they were dispatched wrong. One narrow task, pre-fetched content, exact fix method, bounded scope, independent verification, direct-work fallback.
- Direct PHP/FTP work always delivered; delegation often didn't. Default for content fixes is doing it directly.

## Files changed
- `~/.hermes/scripts/adsense-auditor.py` — added 5 policy-based checks
- `~/.hermes/skills/wordpress/adsense-quality-debug/SKILL.md` — CEO dispatch rules
- `~/.hermes/skills/promotion/ceo-of-domains/SKILL.md` — CEO dispatch rules
- Live sites: howzitza, zadocs, whippetqr nav "Articles" links

## Verification
- Auditor re-run (report-10.json): all 7 sites 0 FAILs
- Nav links verified rendering on live pages
