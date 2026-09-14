# 2026-09-14 — Revert of the writing-style fix (d572e13)

## Why this happened
The user repeatedly said the generated Clash winner posts were "nonsense talk, weird
sentence structure, too verbose — not what we had previously."

Two wrong turns by me:
1. `b0940d8` — injected the per-event `winner-announcements-writing.md` into
   `build_prompt` ("EVENT WRITING STYLE" block). **Not part of the code that
   produced the 19 approved V&S posts** (those were generated 2026-08-30 with
   neither the injection nor any recipe edit).
2. A full rewrite of the shared 462-line recipe to a 124-line "tight spec" — the
   opposite direction; it made output MORE stiff and bureaucratic.

## What the evidence showed
- The 19 approved V&S posts: 48–85 words, 2–4 sentences, 1–2 short paragraphs,
  plain warm voice, fact-first opening ("A wonderful result for {Producer} at the
  {Event}. Their {product} has been awarded {award}.").
- Both V&S and Clash ran the SAME code path; the prompts were byte-identical after
  normalising facts. The bad voice came from my recipe rewrite + the injection,
  not from any Clash-specific config.

## The revert (commit d572e13)
- `content/services/prompt_builder.py` → `git checkout ad75791` (the version that
  made the approved posts): NO `load_style_file`, NO "EVENT WRITING STYLE". Verified:
  grep for both = 0 hits.
- Recipe files restored to the ORIGINAL 462-line version:
  - `/srv/ai/factory/content-studio/03-award-winner-announcements.md`
  - `/srv/ai/programmes/aurora/content-studio/03-award-winner-announcements.md`
  Verified both = 462 lines, 0 of my rewrite markers ("Length — Hard Requirement",
  "Opening Must Name The Winner First", "bureaucratic vocabulary").
- Backups kept: `.bak` (original), `.bak2` (state after my earlier "Opening" patch).

## Lesson (recorded in skill)
- "Don't redo what we did previously / we had a recipe, do not rewrite it." The
  approved voice comes from the ORIGINAL 462-line recipe + ORIGINAL prompt_builder.
  Do NOT inject style files or rewrite the recipe to "improve" the voice.
- To produce winner posts the user approves: revert to that state, run generation
  in the BACKGROUND (poll the log) so the turn doesn't abort, and show the user a
  real sample to approve BEFORE mass-generating.

## Live state after revert
- Campaign "Clash 2025 Winners Feature" (id 3): 35 winners featured.
- ContentItems in campaign 3: post 147 (Goedverwacht, from the pre-revert recipe,
  status draft) — should be deleted/regenerated to confirm the reverted voice.
- No approved Clash posts yet. Next step: generate a couple in the background,
  show the user, get approval, then mass-generate.
