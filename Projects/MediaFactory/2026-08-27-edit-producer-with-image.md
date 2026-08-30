# Media Factory — Edit Producer Info With the Image (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## What changed
The **Asset edit page** now edits the linked participant's info together with the image. When an asset is tied to a winning product (via `winning_products` related_name), the edit form shows:
- Producer / Name
- Product
- Award
- Category
- Website
- Facebook
- Instagram
- Hashtags

Saving persists to the participant AND syncs the asset title. So a producer change (or product rename) is now done right at the image.

## Why
User needs to edit imported info (e.g. product name with junk suffix like "- 8YO", or a producer rename) and wanted to do it where the image is, since the info is tied to the image.

## Verification
- Asset edit page (`/assets/<id>/edit/`) shows all winning-product fields for a participant-linked asset.
- POST saves to participant and syncs asset title (verified: hashtags changed, title synced).

## Note
The info is stored on the **Participant** record (which owns the asset). Generation reads from Participant directly. Editing via the asset page updates the same record.
