# DESIGN SESSION — Winner-announcement copy that a human would be happy to publish

**Date:** 2026-09-14 · **Programme under test:** Clash of the Cultivars 2026 (35 winners)
**Constraint from owner:** "We will for now have to work with gemma." → local Ollama only.

---

## 1. The problem, stated precisely

There are three states the copy has been in, and the owner has rejected two of them:

| State | What it reads like | Verdict |
|---|---|---|
| A. Pre-AI deterministic | "Congratulations to Benguela Cove Wine Estate for being awarded a Gold for their Benguela Cove Lighthouse Sauvignon Blanc 2026. Visit their Website at … Like them on Facebook at … Visit them on Instagram at … #tags" | **Fallback, not the goal.** Owner: *"That is what we had before AI, now you do not want to use AI to do it better?"* |
| B. Two-stage AI (writer 26b → checker 12b) | "A lovely bit of news… a standout result… reflects a high level of skill… in the top tier of its class" | **Rejected** — cringe, verbose, bureaucratic |
| C. Deterministic interim (current, ids 192–226) | Identical sentence for all 35 winners, only names swapped | **Accepted as interim, explicitly "boring"** |

So the target is a fourth state: **the factual base of A, raised by an AI that adds genuine warmth and human rhythm — without B's cringe.**

---

## 2. The clue the owner pointed at (and it is the right clue)

The owner asked: *"What about the md files for the date reminders, will that not help to give you a clue?"*

**Yes — and it is the whole answer.** Date reminders are the ONE content type in this machine whose AI copy the owner has accepted. `_generate_reminder_copy()` in `content/services/media.py:496` produces:

> "Please ensure your samples are delivered to Bontevlei Venue on 07 September 2026… Remember to include your current WSR2 or WSR4 certificate."

That pipeline works, and here is exactly how it differs from the broken winner pipeline:

| Working date-reminder pipeline | Broken winner pipeline |
|---|---|
| **Loads the event's writing-style md** (`date-reminders-writing.md`) and puts it in the prompt | **Never loads** `winner-announcements-writing.md` — the file exists per event but is dead weight |
| **ONE model call** | **TWO calls**: writer 26b (0.75) then checker 12b (0.15) |
| **Structured JSON output** (`{"on_image": …, "post_caption": …}`) | Free prose — nothing bounds the shape |
| **Hard length rule** ("1–2 minimal sentences", "2–4 short sentences") | **No length constraint at all** — the recipe's 130–200w block was gated off, leaving nothing |
| **Temperature 0.5** | 0.75 writer → 0.15 checker |
| **Deterministic fallback** if the model fails | No fallback |

**Diagnosis: the winner pipeline is the only AI pipeline in the machine that skips its own style file and then runs a second model whose job is to sand the voice flat.** The checker (`checker_system`: "Do not sterilise the writing") does the opposite of what it says — a 12b model at temp 0.15 rewriting a 26b draft is a *lossy recompressor*, and what it preserves is the safe, generic, bureaucratic register. Every rejected line traces to this pair.

The 490-line recipe was never the problem. It already says "Opening Must Name The Winner First", "Avoid Empty Fluff", "Congratulations is often appropriate but not mandatory". It is a good spec. Nothing was executing it.

---

## 3. Design decision

**Rebuild the winner-copy step to be structurally identical to the working date-reminder step.**

1. **Delete the checker pass.** One model call. The checker is the flattening agent; a fact-guard prompt is the wrong tool for prose quality.
2. **Inject the event's `winner-announcements-writing.md`** (ai_style asset → programme → factory), exactly as date reminders do.
3. **Demand structured JSON output** with a fixed schema, so the model fills slots instead of free-associating.
4. **Hard length budget** in the prompt (the recipe's own "would a short announcement be stronger?" made numeric).
5. **Keep the deterministic social footer** built from data — never let the model touch URLs.
6. **Keep a deterministic fallback** so a model failure can never produce an empty post.
7. **Anchor on the owner's own reference post** as an in-prompt example (few-shot), because the target voice exists and has been shown to us.

### What stays (non-negotiable, already settled in RULES.md)
- Producer + product + award named in the **first sentence**. Deterministic, can never be violated.
- Social footer inline from participant data ("Visit their Website at … Like them on Facebook at … Visit them on Instagram at … #tags").
- One shared generator machine-wide (`generate_winner_announcement`).
- No invented tasting notes, ingredients, URLs, judging mechanics.

---

## 4. The variants to test (why each one)

Since gemma is the constraint, the levers available are *prompt architecture*, not model quality. Each variant isolates one lever so we can tell which one actually moves the prose.

| # | Variant | Lever isolated |
|---|---|---|
| A | **Control** — current deterministic interim | baseline, for side-by-side |
| B | **Single call + writing md + JSON + length cap** | removes checker; adds style file (the date-reminder recipe applied verbatim) |
| C | **B + few-shot voice anchor** (owner's Benguela Cove post + 2 approved V&S posts) | does showing the target beat describing it? |
| D | **C + explicit anti-patterns** (the exact rejected cringe lines, banned) | does naming the failure mode suppress it? |
| E | **Hybrid** — deterministic fact-first opener preserved byte-for-byte, AI writes only the second beat | guarantees rule #1 while still getting AI warmth |
| F | **B on qwen3:30b-instruct** (already on the box) | is gemma itself the ceiling? one local model swap |

Tested against **3 real winners** chosen to cover the data edge cases:
- **469** Allesverloren Estate Wines / Double Gold / Allesverloren Cape Vintage — clean, and its IG URL carries tracking junk (`?igsi=…&utm_source=qr`)
- **470** Goedverwacht Wine Estate / Gold / Great Expectations Chenin blanc — has a category ("16. Chenin blanc Unwooded") that genuinely adds context
- **477** (or nearest) a winner whose product string repeats the producer name ("their LouisVale Merlot") — the awkward-prefix case

---

## 5. How we will judge

Against the owner's own acceptance bar, not my taste:
- Does sentence one name producer + product + award, plainly, before any praise?
- Does it read like a person wrote it, or like a database with adjectives?
- Is it **shorter** than the rejected version, not longer?
- Would the owner post it as-is?
- Zero invented facts (tasting notes, judging mechanics, URLs).

The owner picks. One sample approved first, then mass-generate — never churn all 35 again.
