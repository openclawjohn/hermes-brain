# 2026-09-14 — Winner announcements: the formula, LOCKED

**Project:** Media Factory · **Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## TL;DR — resume here

The winner-announcement copy formula is **locked** and **wired into the live app**.
It works for all four events and any future event. We will revisit the *voice* when a
stronger local AI lands; the *structure and rules* are settled.

**Authoritative files:**
- Formula (read this first): `/srv/ai/factory/content-studio/winner-announcements-formula.md`
  — also copied to every programme's `content-studio/` folder.
- Code: `content/services/sentence_engine.py` (structure + rules + hygiene)
- Code: `content/services/warm_lines.py` (every phrasing pool — **the file to improve**)
- Live entry point: `content/services/winner_announcements.py::build_winner_body`

## What the owner said, in their words

1. *"The sentences are still terrible, with sentences starting with 'A Gold', 'Gold for',
   instead of 'This company earned a Gold for'."* → **sentence one always starts with the
   producer as the subject.**
2. *"Never give out stats… the drinks category produced 39 awards this year. That is
   proprietary information."* → **no statistics, ever.**
3. *"That is proprietary information, not to be disclosed."* → same rule, absolute.
4. *"You are whinching because you do not have imagination… how wonderful double gold is,
   what a nice award silver is."* → **write with warmth; the tier's meaning matters.**
5. *"Never use the word competition — always use the word event. Make this a rule always."*

## The three-sentence structure

| # | Owns | Source |
|---|---|---|
| 1 | Producer (as subject) + award + product + event | database, deterministic |
| 2 | What the award **means**, matched to the tier | composed pools |
| 3 | What the craft demands **and** what it means to the team | composed pools |

## Why composition, not lists

Two problems were found by generating real posts for all four events:

**The Clash formula does not transfer.** Clash used "one of six awards this producer
collected". Aurora (137) and SAFB (127) have **no multi-award producers at all** — 264 of
306 posts come from producers who won exactly one award. Award counts are useless there.

**A finite pool exhausts.** 32 Double Gold lines cannot serve 72 Double Gold winners. First
attempts produced 62 single-sentence Aurora posts and only 11 distinct second sentences
shared across 126 posts — the old formula with extra steps.

**Fix:** sentences 2 and 3 are *composed from two independent clause parts*, so capacity
multiplies instead of running out:
- sentence 2 = 20 openers × 12 closes = **240** Double Gold, **192** Gold sentences
- sentence 3 = 4–6 craft clauses × 14 meaning clauses × 3 connectors = **252** per archetype

## Verified results (live participant data)

| Event | Posts | Dropped | Distinct 2nd sentences | Stats leaked | Bad openings |
|---|---|---|---|---|---|
| Aurora 2026 | 137 | 0 | 137 | 0 | 0 |
| SAFB 2026 | 127 | 0 | 127 | 0 | 0 |
| Clash 2026 | 35 | 0 | 35 | 0 | 0 |
| V&S 2026 | 22 | 0 | 22 | 0 | 0 |

Verified via the **live app function** `build_winner_body()`, not a test script only.
`manage.py check` passes.

## Real data defects found and fixed

- Corrupted Instagram URL: `sihttps://…/silkbush_wines/?hl=enlkbush_wines` (was live in post 223)
- Tracking junk: `…/allesverlorenwine?igsi=…&utm_source=qr`
- Product repeating the producer (`Allesverloren Cape Vintage`), and a leading comma left
  behind by the strip (`, Lemongrass & Ginger Tea`)
- Numbered admin categories (`91. Cape Vintage Port`), parenthetical categories
  (`Drinks (Alcoholic (wine etc.) and non-alcoholic)`)
- `The FMC` must **not** lose its leading "The" — it is part of the wine's name
- Double space inside a producer name (`Nutris  Wellness Group`)
- Wrong possessive (`Vineyards's` → `Vineyards'`)
- A post listing **its own wine** as another winner that "also placed"
- A sparkling honey **drink** described as wine (classifier now runs on the judged
  category, most-specific-first, wine last)

## Known limitation — what to improve next

Every post is 47–69 words and **structurally identical**: fact → what the award means →
what the craft demands. The sentences vary and none repeat, but the *shape* does not.

Warmth comes from fixed phrasing pools, not from a model that understands the product.
That is the gap the better local AI closes. **The architecture is built for exactly that
swap:** `warm_lines.py` holds all phrasing — improve that file only, nothing else changes.

## Tested and REJECTED — do not retry

- **A second AI pass** (`gemma4:12b` "checker" rewriting the `gemma4:26b` draft) — it
  flattened the voice into bureaucratic filler and produced every cringe line the owner
  rejected ("a lovely bit of news", "a standout result", "in the top tier of its class").
- **A free-form AI third sentence** — gemma invented winemaking technique ("requires
  precise acid management") or restated sentence one; across six products it produced the
  same sentence five times.
- **AI warmth for single-award producers** — produced "It is lovely to see such hard work
  pay off like this" in 3 of 5 posts. Same cringe register.

`gemma4:26b` has a **measured ceiling** on warm prose. A prompt will not move it.

## Also fixed this session (unrelated to copy)

Auxiliary models that had been retired by the provider were causing repeated
"Interrupted during API call" failures — the compression model's HTTP 410 meant context
never actually shrank. Replaced: `kimi-k2.5`→`kimi-k2.6` (compression),
`gemma3:12b`→`gemma4:31b` (titles), `gemini-3-flash-preview`→`kimi-k2.6` (web extract),
`devstral-small-2:24b`→`gpt-oss:20b` (skills hub), `nemotron-3-nano:30b`→`gpt-oss:20b` (mcp).
