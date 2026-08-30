# Media Factory — Image Factory Built (2026-08-27)

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
12. **Built Image Factory** — Pillow-based branded award image generation. Verified end-to-end: generated a 1080×1080 "Double Gold" image via the web UI. 191 product images registered for Aurora 2026.
13. **Added** PROJECT_STATE.md, DESIGN_SYSTEM.md, RULES.md.

## Key decisions

- **SQLite for V1** (single-user internal tool), integer PKs.
- **Bootstrap kept** for legacy template compatibility; `app.css` overrides shared names.
- **Focused core scope** (per user): Programmes/Events + Campaign Planner + Content + CRM + Dashboard + design system + AI engine + Image Factory. Publishing Engine (Postiz), Analytics/Automation deferred.

## Verification

- `manage.py check` passes.
- All 20 key routes HTTP 200.
- AI engine generated real content via local Ollama.
- Image Factory generated a real branded image via the web UI.
- Visual screenshot of the app pending (browser remote-debugging approval not granted).

## Note on Image Factory

The source product photos already contain the Aurora Challenge award seal baked in (top-right). The factory's bottom banner is therefore redundant for these specific images. The factory is functional; the design can be refined to detect/avoid double-branding.

## Outstanding

- Visual verification of the app design.
- Next passes: Publishing Engine (Postiz), Analytics/Automation.
