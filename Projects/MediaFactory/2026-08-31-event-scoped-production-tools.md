# Media Factory — Event-Scoped Production Tools (2026-08-31)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm` · Commit `9b79780`

## What & why
The user pointed out that **Image Factory and Brand Studio are event-scoped tools** (their data models are tied to an event/programme), but the sidebar treated them as global — so the sidebar links "didn't do anything" useful. Document Factory isn't needed now. The fix: put the event-scoped tools on the **event detail page** (the hub), and remove the misleading global sidebar links.

## What changed
- **Sidebar Production group** now only has **Publishing**. Removed:
  - Image Factory (was pointing at the general library, not the event factory)
  - Brand Studio (was a global list, not event-scoped)
  - Document Factory (not needed now)
- **Event detail page** now has two new cards:
  - **🖼️ Image Factory** → `/assets/event/<slug>/<year>/image-factory/` (event-scoped)
  - **◍ Brand Studio** → `/brand/event/<slug>/<year>/` (event-scoped: upload brand assets + analyse visual identity with local AI)

## Verify
- Sidebar no longer shows Image Factory / Brand Studio / Document Factory; Publishing remains.
- Event detail page (Clash 2026) renders both new cards with correct event-scoped hrefs (HTTP 200).
- `manage.py check` passes.

## Notes
- Document Factory (award certificates) still exists in code/routes but is no longer linked in the nav — it can be re-added when needed.
- The global `/brand/visual-profiles/` page (with the "Analyse with AI" link) is still reachable via the event's Brand Studio page if needed, but the primary path is now event-scoped.
