# Media Factory — Unmissable AI Generation Overlay (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Request
The "Generating your post…" indicator was just text and barely noticeable while AI runs.

## Change
Upgraded the AI-generation loading overlay on BOTH `templates/content/generate.html` and `templates/assets/event_library.html` (single source: CSS in `static/css/app.css`).

New visuals:
- Dark blurred full-screen backdrop (`rgba(3,7,18,0.72)` + backdrop-blur) that appears over the whole page.
- Big 84px dual-ring spinner (6px) that spins + an outer counter-rotating ring.
- A pulsing "ping" ring (`gen-ping`) radiating outward.
- Animated shimmer gradient on the title ("Generating your post").
- Three bouncing dots below the title (`gen-bounce`).
- Card pops in with a spring ease (`gen-pop`); backdrop fades in.

## Files
- `static/css/app.css` (`.gen-loading*`, keyframes gen-spin/ping/fade/pop/shimmer/bounce)
- `templates/content/generate.html`
- `templates/assets/event_library.html`

## Verify
- CSS served at `/static/css/app.css` contains the new keyframes (grep count of gen-shimmer/ping/bounce/pop = present).
- base.html loads `css/app.css`.
