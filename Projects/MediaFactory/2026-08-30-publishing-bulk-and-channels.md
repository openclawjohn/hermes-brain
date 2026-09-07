# Media Factory — Publishing Bulk Select + Postiz Channel Setting (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## 1. Publishing queue now supports bulk select
The "Queue Approved Content" card on the Publishing page now lists **all approved posts as a checkbox table** with a select-all toggle. Tick one or many → choose platform → **Queue Selected** → all become Publishing Jobs. Individual queueing is still covered (per-row).

## 2. Postiz channel setting is now visible (was hidden)
The Postiz Instagram/Facebook channel IDs were on `ProgrammeForm` but **no template rendered that form** — so the user couldn't find where to set them. Added a **"Programme Settings — Publishing Channels"** card to the programme detail page (`/programmes/<slug>/`) with fields for both Postiz IDs + a Save button. New `detail()` view handles the settings form separately from the "Add Year" form.

## Where to set the V&S channel
`Programmes` → **Vine and Spirit Awards** → scroll down → **Programme Settings — Publishing Channels** → paste the Postiz Instagram + Facebook integration ids → **Save**.

## Note
Only Aurora currently has channel IDs set. V&S must have its Postiz ids configured before publishing will succeed, or `create_scheduled_post` raises "No Postiz integration configured for programme '...' on platform '...'".

## Verified
- Programme page renders the settings card with both Postiz fields.
- Publishing page renders the bulk-queue table (aqCheckAll, Queue Selected).
- `manage.py check` passes.
