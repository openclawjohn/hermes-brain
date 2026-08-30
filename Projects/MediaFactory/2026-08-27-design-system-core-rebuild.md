# Media Factory — Design System + Core Rebuild (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` (pushed to Gitea)

## What was done

1. **Audited** the project — found the working tree was a broken half-rewrite that gutted a working app.
2. **Preserved** the broken rewrite to `wip/broken-rewrite-snapshot` (pushed to Gitea).
3. **Restored** the proven baseline `33aa91c` and verified the app runs.
4. **Verified Gitea** — running, reachable, push works.
5. **Built the design system** — `static/css/app.css` (Inter, design tokens, sidebar, badges, cards, tables, buttons, forms) + new `base.html` with left sidebar (brief C.3 order).
6. **Rebuilt the dashboard** — KPI cards + programme/event management.
7. **Wired campaigns** to programme/event with create/edit views.
8. **Added Producer CRM** — new `producers` app (Producer, Contact, Product) with full CRUD.
9. **Redesigned** programmes, editorial calendar, event detail hub.
10. **Re-seeded** 5 programmes + 6 events after DB reset.
11. **Added** PROJECT_STATE.md, DESIGN_SYSTEM.md, RULES.md.

## Key decisions

- **SQLite for V1** (single-user internal tool), integer PKs.
- **Bootstrap kept** for legacy template compatibility; `app.css` overrides shared names.
- **Focused core scope** (per user): Programmes/Events + Campaign Planner + Content + CRM + Dashboard + design system. AI/Image/Document/Publishing deferred.

## Verification

- `manage.py check` passes.
- All key pages HTTP 200: `/`, `/programmes/`, `/campaigns/`, `/producers/`, `/calendar/`, event-scoped content/assets/brand/knowledge/publishing.
- Visual screenshot pending (browser remote-debugging approval not granted).

## Outstanding

- Visual verification of design.
- Redesign remaining legacy templates (content, publishing, assets, brand, library).
- Next passes: AI Editorial Engine, Image/Document Factory, Publishing Engine, Analytics/Automation.
