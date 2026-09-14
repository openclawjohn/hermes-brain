# 2026-09-14 — Intuitive winner-feature campaign flow

## Problem
The user uploaded 35 Clash of the Cultivars winners (details + images) and wanted a campaign that features them, but couldn't work out how. Root causes found:

1. **A `Campaign` was a static metadata shell.** It had `programme/event/type/template/name/dates/status` but **no link to winners (Participants)** and generated nothing. "A campaign that features winners" had no path to completion.
2. **The actual winner-announcement generator was buried in Assets** as a bulk action — not discoverable from any campaign flow.
3. **Clash (and 3 other programmes) had NO Winners campaign template.** The "Generate Campaign" page showed **"No active templates"** for Clash, because only Aurora had a Winners type/template. This is the exact wall the user hit.

## Fix (programme-independent, one machine one generator)
- **`Campaign.participants` M2M → `library.Participant`** (migrations 0004 + 0005). The missing link between a campaign and its winners.
- **New shared service `content/services/winner_announcements.py`** — `generate_winner_announcement(event, participant, asset=None, content_type=..., platform=...)` produces ONE winner post (AI copy via `build_prompt`+`generate`, composite overlay via `find_award_overlay`+`composite_award_overlay`, media asset, draft ContentItem) and is idempotent (skips if a post already exists). Both the Assets bulk-announce flow and the campaign flow use it — **one generator, machine-wide**.
- **Campaign detail page = the intuitive hub** (`templates/campaigns/detail.html`):
  - flat-row list of the event's winners (thumb, producer/product, award, **generated-status badge**);
  - bulk-select + "**Generate selected winner posts**" button routing through the AI-busy spinner (`common_ai_busy.html`);
  - per-winner **Preview** → native `content-detail` (image + text) where the user can **Approve** (draft→approved, exact `content-approve` path);
  - "Add more winners" multiselect (participants not yet featured);
  - generation result flash (`campaign_result`).
- **New views/routes:** `campaign-add-participants`, `campaign-generate-winners-busy` (spinner), `campaign-generate-winners` (worker, POST).
- **Auto-feature on Winners-type generate:** creating a campaign from a template whose `campaign_type.name` contains "winner" auto-features every active event participant.
- **Seeded a Winners CampaignType + "Winner Feature" template for Clash, Gold Wine, SAFB, V&S** (Aurora kept its existing "Winner Launch"). So "Generate Campaign" is now a real path for every programme.

## Verified
- `manage.py check` clean; migrations applied; `makemigrations --check` = no changes.
- Test-client: campaign detail 200 with all markers; add-participants 302 + 2 featured; busy spinner page 200 with auto-submit form; POST worker 302 → detail + correct flash ("Generated 0 winner post(s); 1 already existed.").
- Real single-winner generation (AI): Clash participant 469 Allesverloren → ContentItem 143, draft, overlay image id 511; visually inspected the composite — clean Clash / Double Gold seals, no third-party watermark.
- Live server (8899) campaign 3 "Clash 2025 Winners Feature": 35 featured, generate button, Allesverloren row shows Generated + Preview link.
- Real campaign 3 exists with `Winners` type + `Winner Feature` template.

## Files changed / committed
- `campaigns/models.py` (+participants M2M)
- `campaigns/views.py` (detail, add_participants, generate_winners_busy, generate_winners)
- `campaigns/urls.py` (+3 routes)
- `campaigns/migrations/0004_campaign_participants.py`, `0005_alter_campaign_participants.py`
- `content/services/winner_announcements.py` (new shared generator)
- `templates/campaigns/detail.html`
- Commits `1ff4ee4` (feat) + `b4470ef` (fix flash), pushed to Gitea.

## Notes / next
- The other 3 programmes' winner campaigns were NOT created (only the template). To use: Event → Generate Campaign → Winner Feature → auto-features the event's winners → generate posts → preview/approve → queue in Publishing.
- The `03-award-winner-announcements.md` content-studio prompt exists at factory level; Clash has no per-programme override (uses shared) — fine.
