# 2026-09-14 — NEXT PROJECT: fix the winner-announcement copy (voice is still "boring")

## TL;DR for future-me
Winner posts are currently **deterministic and factual** (`"Congratulations to {Producer} for being awarded a {Award} for their {Product} {Year}."` + inline socials). The user accepted this as an interim, explicitly to fix the voice LATER. This note is the handoff for that project. **Do not reinvent the wheel — the recipe already says what good copy is; the blocker is the LLM's prose quality + a broken pipeline, not a missing spec.**

## The one thing that kept breaking it (root cause, finally identified)
The winner copy pipeline was a **two-stage LLM pass**:
1. `gemma4:26b` writer (temp 0.75) — wrote the body.
2. `gemma4:12b` checker (temp 0.15) — a "final edit" pass that **flattened the voice into bureaucratic filler** and added generic closing lines.

Every "cringe" post the user rejected ("a lovely bit of news", "a standout result", "reflects a high level of skill", "in the top tier of its class") came from this pair — the recipes were never the problem.

## What's true about the recipe (verified, not assumed)
- Factory + Aurora `03-award-winner-announcements.md` are **IDENTICAL (490 lines)** and carry **"Opening Must Name The Winner First"** — first sentence MUST state producer + product + award plainly; "Never open with abstract praise"; fact-first example shapes given. F&B (safb) has no own content-studio → resolves to the shared factory recipe.
- Recipe also has: "Avoid Empty Fluff" (bans "a testament to", "significant achievement", "world-class"…), "Congratulations is often appropriate but not mandatory", "Calls To Action optional — never invent URLs", "no breathless advertising".
- The recipes live under `/srv/ai` — **NOT in any git repo**. Only on-disk variants: `.bak`/`.active-462` (original 462-line, no Opening section), `.bak2`/current active (490-line, with Opening section). Nothing in Gitea/history has an "even better" recipe — I searched every branch.

## What the user actually wants (from THEIR words + the Benguela Cove example they pasted)
- The model reference for the target voice:
  > "Congratulations to Benguela Cove Wine Estate for being awarded a Gold for their Benguela Cove Lighthouse Sauvignon Blanc 2026. Visit their Website at https://www.benguelacove.co.za/ Like them on Facebook at … Visit them on Instagram at … #benguelacove #goldawards"
- Their key correction: **"That is what we had before AI, now you do not want to use AI to do it better?"** — i.e. plain boring deterministic copy is NOT the end goal; it's the fallback. They want AI to take a clean factual base and make it **warm, human, worth reading, still fact-first**, NOT cringe/verbose.

## Why the naive "AI second sentence" fix failed (recorded so you don't repeat it)
I tried adding ONE AI-written warm follow-up sentence after the deterministic lead. gemma4:26b produced:
- "It is so wonderful to see such hard work being recognised like this." (clunky, ungrammatical-ish)
- "I am so thrilled to hear that the Great Expectations Chenin Blanc took home the gold!" (slang)
Tightening the instruction, banning filler words, did NOT fix it. **Conclusion: gemma4:26b's ceiling on polished human prose is the blocker, not prompt wording.**

## Real paths for the next project (pick deliberately, don't guess)
1. **Swap WRITER_MODEL to a stronger API model** (e.g. a capable hosted LLM) and keep the structure (deterministic fact-first lead + AI warm line + data footer). Best copy; needs user's call on provider/model/cost. This is the only path that gets true "AI does it better" copy.
2. **Hand-edit the warm line before publish** (human-quality, not autonomous) — keep gemma4 for structure, a human/agent tunes the line.
3. Keep deterministic (current) — what the user called "boring / before AI"; fine as fallback only.

## Structural things that MUST stay (do not regress)
- Deterministic **fact-first opener** built from participant data — can never violate "name producer+product+award first".
- **No AI-written second sentence** until the model is upgraded (commit ac986a0 cut it).
- Data-built social footer inline: "Visit their Website at … / Like them on Facebook at … / Visit them on Instagram at … / {hashtags}".
- The generator: `content/services/winner_announcements.py::generate_winner_announcement` — THE single machine-wide path. Commit `ac986a0`.

## Known data quirks to fix when polishing copy (source data, not template bugs)
- Some participant **Instagram URLs carry tracking junk**: `?igsi=...&utm_source=qr` (e.g. Allesverloren). Clean these before publishing.
- Some **product names read awkwardly** because the product string already contains the producer name or "The": "their The FMC", "their Long Dog Wines Tannat", "their LouisVale Merlot". Might need a rule to drop the producer prefix when product starts with it.

## Current live state (so a new session can pick up)
- Clash 2026 winner posts: **35 deterministic drafts, ids 192–226**, in the Content Library at `/content/clash-of-the-cultivars/2026/library/`. Status = draft, awaiting review.
- **No queue exists yet.** Approving alone does NOT queue a post (approval just flips status). To publish: `/publishing/clash-of-the-cultivars/2026/` → tick posts → platform → create queue (drafts auto-approve on queue). Randomise button is on the Publishing page queue row.
- **Posting window:** every event is fixed 08:00–16:00; scheduler clamps slots back to the event window, overriding queue stop_time >16:00. User wants next queue 08:00–15:58, all posts in one day (start=stop same date, frequency 1/blank).
- Campaign "Clash of the Cultivars 2026 — Winner Feature" (id 5), 35 winners, planning 2026-09-14 → 2026-10-13.
- Commits: `ac986a0` (deterministic copy), `b236179` (docs), all on `feature/design-system-crm`, pushed to Gitea.

## Lesson on process (so we don't churn like today)
- The 19 approved V&S posts were flagged by the user as ALSO cringe — do NOT treat old approved posts as the gold standard; the recipe + the user's example are the source of truth.
- "Award must be mentioned first" = producer + product + award all in sentence one — already in the recipe, already deterministic.
- The user values programme-independent fixes and "do not reinvent / use the md files" — always read the existing recipe first and enforce it before inventing.
- Show ONE real sample and get approval before mass-generating (I regenerated all 35 twice churning).
