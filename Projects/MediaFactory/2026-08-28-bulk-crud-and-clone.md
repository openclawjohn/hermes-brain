# Media Factory — Bulk/Edit/Delete Recon + Event Clone (2026-08-28)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Recon: every list now has bulk-select + edit/delete
The user asked for bulk choose/edit/delete on every list. Coverage after this session:

| List | Select | Bulk actions | Per-item Edit/Delete |
|------|--------|--------------|----------------------|
| Assets | ✅ select-all + count | Bulk announce winners, bulk delete | ✅ |
| Participants | ✅ | Deactivate, delete | ✅ |
| Content Library | ✅ | Approve, set-to-draft, delete | ✅ (Open/Delete) |
| Producers CRM | ✅ (new) | Delete selected | ✅ (Edit; detail has more) |
| Knowledge items | ✅ (new) | Delete selected | ✅ |
| Image Factory | ✅ (new) | Delete selected (generated) | ✅ (Edit/Delete per asset) |

## Image Factory fixes
- **Bug:** `product_images` filtered on `purpose="product_image"`, but V&S winners are `purpose="other"` → dropdown was empty, nothing selectable.
- **Fix:** include both `product_image` and `other` purposes. Now all 24 V&S product images are selectable.
- **Added** bulk select + delete on the Generated Assets table, plus per-asset Edit/Delete (reuses `asset-edit`).

## Event clone (factory-correct, no hardcoding)
New `event_clone` view + `/programmes/<slug>/event/<year>/clone/`. Cloning an event into a new year carries forward:
- **Milestones / date reminders** (dates shifted by year difference).
- **Reusable assets** — templates, overlays, branding, backgrounds (files copied; product images/winners NOT — those are year-specific).
- **Standard knowledge types** (Judges, Participants, Partners, Editorial Guide, FAQs).
"Clone to New Year" button on the event detail page.

Verified: cloned Vine & Spirit 2026 → 2025, the 4 overlays copied with award mappings + files intact (test clone cleaned up).

## Notes
- All commits pushed to Gitea (`feature/design-system-crm`).
- The user's earlier concern about event-name hardcoding was valid for Postiz IDs — fixed in the prior commit; `event`/`year` params here are generic factory inputs.
