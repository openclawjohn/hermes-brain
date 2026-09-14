# 2026-09-14 — Winner-announcement copy: design session + working pipeline

**Programme used for practice:** Clash of the Cultivars 2026 (35 winners)
**Constraint:** local gemma only (`gemma4:26b`), as instructed.
**Status:** working end-to-end on real data; **nothing in the live app changed yet** — awaiting owner pick.

---

## The diagnosis (finally precise)

Three states of this copy exist and two were rejected:

| | Reads like | Verdict |
|---|---|---|
| A | "Congratulations to X for being awarded a Y for their Z 2026. Visit their Website at…" | accepted as interim, explicitly "boring" |
| B | "a lovely bit of news… a standout result… in the top tier of its class" | rejected — cringe |
| C | identical sentence for all 35, names swapped | current live state |

**Root cause of B, now proven by reading the code:** `content/services/ollama.py::generate()` was a **two-stage** pipeline — `gemma4:26b` writer at temp 0.75, then `gemma4:12b` "checker" at temp 0.15 whose instructions said "do not sterilise the writing". A 12b model rewriting a 26b draft at temp 0.15 is a lossy recompressor; what it reliably preserves is the safe bureaucratic register. Every rejected line came from that pair. Additionally the winner path **never loaded** the event's `winner-announcements-writing.md`.

## The clue the owner pointed at — and it was the right one

Date reminders are the one AI pipeline here whose copy the owner **accepts**. Reading `_generate_reminder_copy()` (`content/services/media.py:496`) shows why:

| working date-reminder pipeline | broken winner pipeline |
|---|---|
| loads its writing-style md | never loaded its writing md |
| **one** model call | **two** calls (writer → checker) |
| structured JSON output | free prose |
| hard length rule | no length rule at all |
| temp 0.5 | 0.75 → then 0.15 |
| deterministic fallback | none |

So the design was: **make the winner step structurally identical to the date-reminder step.** One call, style md loaded, JSON out, length capped, fact footer deterministic, fallback deterministic. That is the whole fix for the cringe.

## What testing then revealed (all with real Clash winners)

Ran 5 variant strategies, then 4 refinement rounds. Real findings:

1. **A single AI call with the recipe + writing md + fact-first rule produces acceptable prose.** Confirmed across 8 attempts: 7/8 clean.
2. **gemma converges on ONE sentence template** ("This result *<verb>* the *<category>* category…"). So 35 posts generated naively all read alike. **Variety cannot come from the model.**
3. Therefore **variety is now deterministic**: 6 validated post frames rotate, so consecutive posts never share a shape.
4. **Category-as-product confusion:** the model wrote "Cape Vintage Port" (the admin category) where "Cape Vintage" (the product) belonged. Fixed by sanitising the category (strip the `91.` ordinal and the "(please specify…)") and labelling it explicitly as *not* the product name.
5. **Invented winemaking detail** was the worst failure mode: "great care with ripeness and structural balance", "across multiple judging rounds". These are specific factual claims about process — forbidden by the recipe. **Fixed with a closed set of observation modes**: the model may only speak about the award tier, the category name, or figures computed from the database. Free-form interpretation is what invented process claims.
6. **Product string duplicates the producer** ("their Allesverloren Cape Vintage") — fixed by stripping a genuine leading repeat only.
7. **The event name already ends in the year** ("Clash of the Cultivars 2026 2026") — fixed.
8. **Instagram URLs carry tracking junk** (`?igsi=…&utm_source=qr`) — stripped.
9. **Two real data facts differentiate posts and are verifiable**: 8 producers won multiple awards (Robertson 6, Gravel Junction 5, Saxenburg 5, Goedverwacht 4, Louisvale 3, Thor 3, Allesverloren 2, Joubert 2), and only 11 of 35 are Double Gold. These became the best lines ("This Gold brings Robertson Winery to six awards earned during this year's competition.").

## The design (final)

Three layers, and the model owns only the middle one:

1. **Deterministic — the frame.** Rotates through 6 validated fact-sentence shapes. Guarantees variety and guarantees producer + product + award appear first.
2. **AI — the observation.** One short sentence (12–22 words), subject drawn from a **closed menu** (award tier / category / computed award counts), validated before use.
3. **Deterministic — the footer.** Built from participant data. The model never touches a URL.

**Validator gate** on everything the model writes: banned-filler list (the recipe's "Avoid Empty Fluff" plus every line the owner rejected, plus ranking claims like "among the most", "one of the best"), no numbered admin codes, no producer restatement, no "This…"/"The result…" openings, one sentence, 8–26 words. On failure it retries (up to 3), and if it still fails the post falls back to the plain factual sentence — **never empty, never invented**.

## Files written (on the server, `/tmp/` — nothing deployed)

- `/tmp/winner_copy.py` — the engine (data hygiene, computed context, frames, validator, closed modes, compose).
- `/tmp/winner_full.py` — the all-35 runner.
- `/tmp/winner_final.json` / `.md`, `/tmp/winner_full.json` / `.md` — real generated output.

Once approved, `winner_copy.py` replaces the copy step inside
`content/services/winner_announcements.py::build_winner_body` so the single
machine-wide generator gains the approved voice — one place, all programmes.

## Open question for the owner

Which output should mass-generate? The 4-winner sample is in
`/tmp/winner_final.json`; the full 35 in `/tmp/winner_full.json`.
