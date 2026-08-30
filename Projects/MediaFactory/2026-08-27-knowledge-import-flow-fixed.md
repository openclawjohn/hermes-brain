# Media Factory — Knowledge Vault Import Flow Fixed (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Problem
User could not see the CSV + images upload functionality anywhere. Wanted to import Vine & Spirit 2026 winners.

## Root cause
The `KnowledgeType` table was **empty**. The Knowledge Vault index (`/knowledge/<slug>/<year>/`) renders one card per `KnowledgeType`, and the "Participants & Winning Products" card is what carries the **"Open" button → participants page → "Import CSV" button**. With zero knowledge types, every event showed "No knowledge types yet" and the import flow was unreachable.

Also confirmed: the earlier Aurora test data (participants) was wiped when the DB was reset during the broken-rewrite restore — so no winner content existed for any event.

## Fix
Seeded the 5 standard knowledge types (DB change, not code):
1. Judges
2. Participants & Winning Products
3. Partners
4. Editorial Guide
5. FAQs

## Verification
- `/knowledge/vine-and-spirit-awards/2026/` now shows the "Participants & Winning Products" card with an "Open" button.
- `/knowledge/vine-and-spirit-awards/2026/participants/import/` renders the full upload form (CSV file + multiple images + Preview Import button).

## Import format (for the user)
The import expects a CSV with columns: `Participant`, `Award`, `Product`, `Category`, `Sub Category`, `content`, `media_url`, plus optional `Website`, `Facebook`, `Instagram`, `Hashtags`. Images are matched to rows by filename (the `media_url` basename must match an uploaded image filename). Social URLs can also be embedded in the `content` column and are auto-extracted.

## Outstanding
- User still needs to provide the actual Vine & Spirit 2026 winners CSV + images to run the import.
