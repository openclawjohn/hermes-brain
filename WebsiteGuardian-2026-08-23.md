# Website Guardian — 2026-08-23 Cycle Report

**All 7 sites HTTP 200** — matches established post-Phase-F healthy baseline (all UP across ~60 consecutive maintenance cycles). No site changed status, no oscillation, no server-side issues.

## Findings

### beanel.com map — FALSE POSITIVE (independently verified WORKING)
Guardian flagged 5 map issues based on source-code heuristics, but live-browser verification (logged-out visitor) proved the map renders correctly:
- **DOM:** `leaflet-container` present, zoom controls + OpenStreetMap attribution present, `L` defined
- **a11y tree:** `button "Zoom in"`, `button "Zoom out"`, `link " Leaflet"`, `link "OpenStreetMap"` all present
- **Tiles:** 8 real OSM tiles loaded (588/589.png), map 858×350px
- **IP display:** shows real `102.132.208.10` (not "Loading...")
- **Vision screenshot:** street/road map centered on Johannesburg with highways (N1/N14/N12), zoom ± buttons, "Leaflet | © OpenStreetMap" attribution, blue location marker

Root cause of false positive: guardian checks curl source HTML for `.leaflet-container` / zoom markers, but those are JS-rendered and never appear in raw source. The skill documents this exact limitation. Map is working.

### howzitza.co.za — FALSE POSITIVE (5 posts)
Guardian flagged 5 posts as "0 unique images," but each actually has 2 unique images (via `/adsense-imgs/` path) and 5,000+ words. Guardian's image check only counted `/uploads/` URLs. **FIXED the guardian** to also count `/adsense-imgs/`. Verified fix works.

Remaining howzitza note: `blog-stereotypes-funny.png` shared across 4 old game pages (IDs 199-220) — pre-existing backlog, not new.

### Pre-existing backlog (not new this cycle, no action needed)
- **sumza.co.za:** 3 featured images shared across 8-12 posts each (sandton-sunrise, solar-heater, uMT4si...jpg); 5 posts have sumza-logo.png ×2 in content
- **zadocs.co.za:** cropped-Second-Logo.png shared across 57 posts (+4 webp variant) — known template-shell backlog
- **whippetqr.com:** featured_media shared on 2 pairs of posts
- **5minutes.co.za:** extensive shared featured_media (media 286-291 shared across 4-5 posts each)

## Action taken
- Fixed website-guardian.py image check to include `/adsense-imgs/` path (git commit `fb3c243`)
- beanel map requires no fix — verified working visually
- No server issues — all sites healthy

## Files
- Guardian script: `~/.hermes/scripts/website-guardian.py`
