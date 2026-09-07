# Media Factory — Clone Buttons Everywhere + V&S Publishing Channels (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## 1. Clone buttons added where logical
The clone capability now appears in multiple places:
- **Programme page** (`/programmes/<slug>/`) — a **Clone** button next to each year, right beside "Open". (This is what the user wanted: they saw "Add Year" there, so Clone should be there too.)
- **Event detail page** — the "Clone to New Year" button (already present).

## 2. V&S Postiz channels fully configured
- Added **Postiz API key** field to the `Programme` model + form (migration `0025`), stored per programme.
- Publishing service reads the API key from the programme (`postiz_api_key`), falling back to env var.
- **Instagram + Facebook channel fields are now dropdowns populated from the Postiz API** — pick by channel name, not paste an ID.
- Set V&S: API key + Instagram (`cmryzuq6q...`, "Vine and Spirit Awards") + Facebook (`cmryzryik...`, "Vine and Spirit Awards"). Verified the API returns 10 channels and both V&S channels resolve.

## Note on the API key & root file
The systemd service's `/etc/media-factory.env` is root-owned (no sudo access for the `m`/hermes user), so the API key can't be set there. Storing it on the `Programme` record is the factory-correct answer — no root needed, works per programme.

## Verify
- Programme page renders Clone next to each year.
- Publishing dropdowns show real Postiz channels; V&S channels pre-selected.
- `manage.py check` passes.
