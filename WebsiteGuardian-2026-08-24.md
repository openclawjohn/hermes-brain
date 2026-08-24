# Website Guardian — 2026-08-24 06:00 SAST

## Status: ALL 7 SITES UP & HEALTHY (maintenance mode)

No server crash, no throttling, no oscillation. Phase 2 throttle-aware subpage/sitemap checks all 200.

| Site | Homepage | Sitemap | Essential | Server |
|------|----------|---------|-----------|--------|
| beanel.com | 200 | — | ✅ | new (164.160.91.56) |
| howzitza.co.za | 200 | — | ✅ | old (164.160.91.40) |
| sumza.co.za | 200 | — | ✅ | old |
| zadocs.co.za | 200 | 200 | 301→200 | old |
| saymyname.co.za | 200 | 200 | 200 | old |
| whippetqr.com | 200 | 200 | 200 | old |
| 5minutes.co.za | 200 | 200 | 200 | old |

## Beanel Map — FALSE POSITIVE (verified working)
Guardian source-code flags (map fix patterns missing, Leaflet init "not executed", IP "Loading...") are **source-code heuristics**, NOT proof of breakage (documented pitfall).

**Live logged-out browser verification (2026-08-24):**
- Leaflet container present, 858x350px
- Zoom in / Zoom out buttons present ✅
- OSM attribution present ✅
- 8 real OSM tiles loaded ✅
- IP `102.132.208.10` displayed (not "Loading...")
- Location grid: Kranshoek / Western Cape / South Africa / Cool Ideas ISP / coords -34.05, 23.37

Map fully working. No fix needed.

## Pre-existing Content-Quality Backlog (NOT fixed this cycle — flagged for attention)
- **howzitza.co.za**: featured image `blog-stereotypes-funny.png` shared across 2 posts
- **sumza.co.za**: `sumza-logo.png` appears 2x in content on 5 posts; 3 featured images shared across 8-12 posts each (sandton-sunrise x8, solar-heater x10, uMT4si0kDN8t9j5tpndX5_ZOgYiGdA x12)

These are image-uniqueness quality items requiring replacement-image sourcing (Wikimedia Commons) + assignment — a content operation, not a guardian auto-fix. Tracked for dedicated content pass.

## Actions taken
- Ran guardian script (timed out at 120s global limit on old-server throttle; Phase 1 partial + Phase 2 throttle-aware recheck covered all 7)
- Live-browser verified beanel map (cleared false positive)
- Logged run to Obsidian + committed
