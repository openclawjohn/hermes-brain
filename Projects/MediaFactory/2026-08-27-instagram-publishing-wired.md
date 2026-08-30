# Media Factory — Instagram Publishing Wired (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` (pushed to Gitea)

## What was done this session (cumulative)

1. **Audited** the project — found the working tree was a broken half-rewrite that gutted a working app.
2. **Preserved** the broken rewrite to `wip/broken-rewrite-snapshot` (pushed to Gitea).
3. **Restored** the proven baseline `33aa91c` and verified the app runs.
4. **Verified Gitea** — running, reachable, push works.
5. **Built the design system** — `static/css/app.css` (Inter, design tokens, sidebar, badges, cards, tables, buttons, forms) + new `base.html` with left sidebar (brief C.3 order).
6. **Rebuilt the dashboard** — KPI cards + programme/event management.
7. **Wired campaigns** to programme/event with create/edit views.
8. **Added Producer CRM** — new `producers` app (Producer, Contact, Product) with full CRUD.
9. **Built Campaign Setup flow** (matches user's diagram) — Campaign Types, Templates, Content Items, Generate (Event + Template = Campaign).
10. **Redesigned ALL templates** to the design system.
11. **Verified AI Editorial Engine** — two-stage Ollama pipeline generated a real 1,070-char educational post.
12. **Built Image Factory** — Pillow-based branded award image generation. Verified end-to-end.
13. **Built Document Factory** — reportlab PDF certificate generation. Verified end-to-end.
14. **Built Publishing Engine** — queue/publish/cancel workflow. Verified end-to-end.
15. **Built Analytics** — operational dashboard. Verified.
16. **Built Automation** — rules with trigger/action, run history. Verified.
17. **Built Notifications** — notification centre with read/unread. Verified.
18. **Built Integrations** — dashboard with live health checks. Verified.
19. **Built System** — health dashboard. Verified.
20. **Redesigned Knowledge Vault templates** — participants, type index, forms, import. Verified.
21. **Wired real Postiz publishing** — Facebook (text) and Instagram (with media asset) both schedule real posts. Verified end-to-end with real post IDs.
22. **Added** PROJECT_STATE.md, DESIGN_SYSTEM.md, RULES.md.

## Key decisions

- **SQLite for V1** (single-user internal tool), integer PKs.
- **Bootstrap kept** for legacy template compatibility; `app.css` overrides shared names.
- **Focused core scope** (per user): Programmes/Events + Campaign Planner + Content + CRM + Dashboard + design system + AI engine + Image Factory + Document Factory + Publishing Engine + Analytics + Automation + Notifications + Integrations + System + Postiz publishing.

## Verification

- `manage.py check` passes.
- All 18 core routes HTTP 200.
- AI engine generated real content via local Ollama.
- Image Factory generated a real branded image via the web UI.
- Document Factory generated a valid, clean PDF certificate.
- Publishing Engine queued + published a content item with timestamp.
- **Postiz publishing scheduled real posts** — Facebook (text) and Instagram (with media) both returned real post IDs.
- Analytics, Automation, Notifications, Integrations, System all verified.

## Outstanding

- Visual verification of the app design (browser remote-debugging approval not granted).
- WordPress integration wiring.
- Entry/judge portals (V2).
