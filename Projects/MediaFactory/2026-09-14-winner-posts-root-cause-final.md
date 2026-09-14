# 2026-09-14 — Winner posts: root cause of "nonsense" copy finally fixed

## The user was right
The two drafts I (and the previous session) produced as "proof" of good writing
style were actually **verbose AI filler**, nowhere near the 19 approved V&S posts.
The earlier session's revert (d572e13) anchored the fix at the WRONG commit and
declared victory prematurely.

## Actual root cause
- The 19 approved V&S winner posts (ContentItem 18–126) were generated **2026-08-30**
  at **62–85 words** with the original `prompt_builder.py`.
- Commit `ad75791` (2026-09-01, "aurora-edu social format") added an **unconditional**
  `SOCIAL-MEDIA FORMAT` block to `prompt_builder.py` that said:
  > "Keep the whole caption between 130 and 200 words" / "Break the body into 2 to 4 short paragraphs"
- That block applied to **every** content type, including `03-award-winner-announcements`,
  and overrode the recipe's explicitly SHORT fact-first voice ("Would a short announcement
  be stronger than a longer one?"). That is what produced the 113–127 word filler drafts.
- The "revert" (d572e13) claimed to restore "back to ad75791" — but ad75791 is exactly
  the commit that introduced the word-count block. So the verbose block survived.

## The fix (commit a4cc1ad)
Gated the `SOCIAL-MEDIA FORMAT` block behind `use_editorial` (i.e. only applied to the
educational/editorial content type it was built for). Winner announcements now get the
original prompt (no 130-200w mandate), so the recipe's short voice applies.

## Verification
- Word counts: old bad drafts 113/119/127 → new post id 155 = **88 words** (V&S target 62–85).
- Regenerated Goedverwacht draft reads like a V&S post:
  > "The Great Expectations Chenin blanc from Goedverwacht Wine Estate has earned Gold at
  > the Clash of the Cultivars 2026. Achieving this result in the unwooded category highlights
  > the skill and precision required to produce a wine that relies entirely on the quality of
  > the fruit..." + link + hashtags.
- Cleaned up test drafts (151–154 deleted). Kept one clean Clash sample (id 155).
- `manage.py check` clean. Pushed a4cc1ad.

## Date-range question (user: "I could not put in the date range")
Proved the edit form **does** accept a custom date range: POST started 2026-10-01 → 2026-10-30
saved + redirected (verified, then restored to 2026-09-14 → 10-13). The dates were never
hand-written anywhere — the template's 30-day `duration_days` anchors defaults to today when
a campaign is generated (commit e5a0083). User wants to set them themselves in the Edit screen;
that works now/unchanged.

## Files
- content/services/prompt_builder.py: gated SOCIAL-MEDIA FORMAT block behind use_editorial.
