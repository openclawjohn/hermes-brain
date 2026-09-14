# 2026-09-14 — Winner Feature: writing style + campaign edit/delete unblocked

## What was done
1. **Verified the writing-style revert is live** (commit `d572e13`). `prompt_builder.py` has zero style-file injection; the winner recipe `03-award-winner-announcements.md` is the original 462-line approved voice. Generated two **real** Clash 2026 winner drafts (ContentItem 152 Allesverloren, 153 Goedverwacht) to prove the voice:
   > "It is rewarding to see the skill and dedication of a producer recognised through independent evaluation. ... The recognition of their Great Expectations Chenin blanc is a wonderful result..."
   Warm, human, producer-facts + link + hashtags. No bureaucratic filler.
2. **Added a campaign Delete flow** (the missing capability — there was NO delete anywhere). New `campaigns/views.py::delete` (GET = confirm page, POST = delete + redirect to Planner), URL `campaigns/<pk>/delete/`, `templates/campaigns/delete_confirm.html`, Delete buttons on the Planner list and campaign detail. Commit `5cf2acb`, pushed to Gitea.
3. **Deleted the leftover practice campaign** "Clash 2025 Winners Feature" (id 3). Verified confirm page (200) → POST (302) → gone; the real 2026 campaign (id 5) untouched with its 35 winners.

## Why the 2025 campaign existed / who wrote the dates
- The whole winner-campaign system was built **today, 2026-09-14** (commits by Louis Nel).
- Campaign dates are **derived automatically** from the template's `duration_days` anchored to *today* (commit `e5a0083`, "derive campaign dates from template duration, not Event"). That's why the 2026 campaign shows 2026-09-14 → 10-13 (30-day template): it was generated today, so the dates anchor to today — **nobody hand-wrote the 2026 dates**, the system computed them.
- The "Clash 2025 Winners Feature" (id 3) was a **practice/duplicate** — same 2026 Clash event, mislabelled "2025" by name, given Nov–Dec dates. It was not a real 2025 year.

## What's editable now
- The campaign **Edit** form already exposes programme/event/type/template/name/description/**start_date**/**end_date**/status. Both campaigns were editable at the HTTP level; the only true blocker was delete, which is now fixed.

## Decisions
- Campaign delete is non-destructive to content: deleting a campaign removes the campaign record only; published/approved content and Content Library items are never touched.
- User keeps only the single real 2026 campaign (id 5).

## Verification
- `manage.py check`: no issues.
- Test client: GET confirm 200 w/ csrf + confirm text; POST 302 → `/campaigns/`; row gone.
- Live autoreload server (`:8899`): `/campaigns/3/delete/` → 200; Planner list rendered both delete links.

## Next (not done)
- Publishing the 2026 campaign's posts to Postiz was NOT verified in this pass (publish_due cron exists; queue flow documented as working). Not required to unblock editing/writing.
