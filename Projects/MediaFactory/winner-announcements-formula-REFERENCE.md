# Winner Announcements — The Formula

**Status:** LOCKED 2026-09-14 (owner: "let's lock this in for now… we will revisit this soon")
**Applies to:** EVERY programme and event that publishes winner announcements —
Aurora, South African Food & Beverage Awards, Gold Wine Awards, Vine & Spirit Awards,
Clash of the Cultivars — and any future programme.
**Authoritative code:** `content/services/sentence_engine.py` + `content/services/warm_lines.py`

---

## The non-negotiable rules

These came from the owner directly. They are not preferences.

1. **Sentence one ALWAYS starts with the producer as the subject.**
   ✅ "108 Peaks has earned a Double Gold for their Almond and Honey Nougat at the Aurora…"
   ❌ "A Gold for 108 Peaks…"  ❌ "Gold for 108 Peaks…"
   These are headline fragments, not English. The owner called them "horrible sentences".

2. **NEVER disclose statistics.** No award counts, no category field sizes, no event
   totals, no "X awards were handed out". This is proprietary information and must never
   appear in a public post. A stats guard scans every generated post and reports zero.

3. **Never use the word "competition" — always "event".**
   Enforced in code (`apply_word_rules`) and stated in prompts.

4. **Three sentences.** Fact → what the award means → what the craft demands and what it
   means to the people behind it. Warm and generous, never breathless or salesy.

5. **Never invent facts.** No tasting notes, flavours, aromas, ingredients, production
   methods, sourcing, judge names, judging procedures, scores or rankings. General
   editorial warmth about what an award *means* is explicitly allowed by the
   organisation's own recipe.

6. **Data-built footer.** The model never touches links:
   `Visit their Website at … / Like them on Facebook at … / Visit them on Instagram at … / #tags`

---

## Why the post has three sentences

| Sentence | Owns | Source |
|---|---|---|
| **1. The fact** | Producer + award + product + event, producer as subject | Database |
| **2. What the award means** | The tier's significance, in warm and varied prose | Fixed tier pools (composed) |
| **3. Craft + meaning** | What it takes to do well in this kind of category, and what the result means to the makers | Category archetype pools (composed) |

Sentence 1 is deterministic — the hard rules can never be violated by a model.
Sentences 2 and 3 are **composed, not picked from a list** (see below).

---

## The two structural problems this formula solves

Both were found by generating real posts for all four events, and both matter because
the four events are NOT alike:

| Event | Winners | Distinct producers | Multi-award producers |
|---|---|---|---|
| Aurora 2026 | 137 | 137 | **none** |
| SAFB 2026 | 127 | 127 | **none** |
| Clash of the Cultivars 2026 | 35 | 13 | many |
| Vine & Spirit Awards 2026 | 22 | 13 | many |

**Problem 1 — the Clash formula does not transfer.**
Clash winners used "one of six awards this producer collected". 264 of 306 posts come from
producers who won exactly ONE award, so award counts are useless there. The formula must
work from the *award tier* and the *category*, not from counts.

**Problem 2 — a finite pool exhausts on big events.**
A list of 32 Double Gold lines cannot serve 72 Double Gold winners — sentences get used up
and posts get dropped. Early attempts produced 62 single-sentence Aurora posts and only 11
distinct second sentences shared across 126 posts: the old formula with extra steps.

**The fix — composition instead of selection.** Each sentence is built from two
independent clause parts:

- sentence 2 = `TIER_OPEN` (20 per tier) × `TIER_CLOSE` (12 per tier) = **240** Double Gold
  sentences, **192** Gold sentences
- sentence 3 = `CRAFT_CLAUSE` (4–6 per category archetype) × `MEANING_CLAUSE` (14) ×
  `CONNECTOR` (3) = **252** sentences per archetype

Nothing runs dry, and a used-set guarantees no sentence is repeated anywhere in a batch.

**Result (verified on live data):**

| Event | Posts | Dropped | Distinct 2nd sentences | Stats leaked | Bad openings |
|---|---|---|---|---|---|
| Aurora 2026 | 137 | **0** | **137** | **0** | **0** |
| SAFB 2026 | 127 | **0** | **127** | **0** | **0** |
| Clash 2026 | 35 | **0** | **35** | **0** | **0** |
| V&S 2026 | 22 | **0** | **22** | **0** | **0** |

---

## Award tiers and their voice

Tier is taken from `Participant.award`. Voice is matched to the tier — never generic.

| Tier | Voice |
|---|---|
| **Double Gold** | The finest result the event gives out. Warm, high. "It does not get better than Double Gold." |
| **Gold** | A strong, respected result. "A real feather in the cap." |
| **Gold & Value** | Quality **and** value together. "A hard pair to get right." |
| **Silver** | A genuinely nice result. Never dismissive. "A nice award to take home." |

`Gold & Value` (used by Vine & Spirit) is written as *"a Gold & Value award"* — the word
"award" is required because of the ampersand.

---

## Category archetypes

Sentence 3 must fit the *kind* of product. The classifier runs on the judged **category**
(never the product name), most-specific-first, because early versions described a
"Sparkling Honey Drink" as wine.

Archetypes: `spirits`, `beer`, `coffee`, `tea`, `frozen`, `dairy`, `bakery`, `oils`,
`meat`, `snacks`, `preserves`, `beverages`, `wine`, `generic` (fallback).

Wine is checked **last** because its pattern is broad. Any unmatched category falls back to
the `generic` pool — never to a wine line.

---

## Data hygiene applied before writing

Real defects found in live participant data, all fixed in code:

| Defect | Real example | Fix |
|---|---|---|
| Corrupted Instagram URL | `sihttps://www.instagram.com/silkbush_wines/?hl=enlkbush_wines` | strip scheme-prefix junk and duplicated tails |
| Tracking junk in URLs | `…/allesverlorenwine?igsi=…&utm_source=qr` | strip the query string |
| Product repeats the producer | `Allesverloren Cape Vintage` from Allesverloren | strip a genuine leading repeat |
| Prefix strip left a comma | `, Lemongrass & Ginger Tea` | strip `,;:` after the cut |
| Numbered admin category | `91. Cape Vintage Port` | strip the ordinal |
| Parenthetical category | `Drinks (Alcoholic (wine etc.) and non-alcoholic)` | shorten for prose |
| Leading "The" in a product | `The FMC` | **never** stripped — it is part of the name |
| Double space in a name | `Nutris  Wellness Group` | collapse whitespace |
| Wrong possessive | `Vineyards's` | names ending in *s* take a bare apostrophe |
| Own wine listed as its own sibling | Tinta Barocca post naming Cape Vintage as "also placed" | dedupe, exclude the post's own product |

---

## Verified output samples

```
108 Peaks has earned a Double Gold for their Almond and Honey Nougat at the Aurora
International Taste Challenge 2026. Double Gold is the finest result this event gives
out, and it is a wonderful thing to achieve. Good baking is a craft of patience and
precision, and it is a proud moment for everyone behind the product.
```

```
Bezalel Estate Cellars has earned a Gold for their VSOP Potstill Brandy at the Vine and
Spirit Awards 2026. A Gold says the judges thought very highly of this product, and a
real feather in the cap. Distilling well is a slow craft that rewards care, and it is a
fine reward for the work that went into it.
```

```
Southern Oil has won a Double Gold for their B-well Canola Oil at the South African Food
& Beverage Awards 2026. This is the top of the tree at the event, and a brilliant outcome
for the product. There is craft in a simple product done properly, and it is a result the
team can be proud of.
```

---

## Honest limitation (known, to revisit)

Every post is now 46–67 words and structurally formulaic: **fact → what the award means →
what the craft demands**. The individual sentences vary and no two repeat, but the *shape*
does not vary.

Warmth currently comes from fixed phrasing pools rather than a model that understands the
product. Closing that gap is exactly what a stronger local AI buys, and the architecture is
built for that swap:

- `sentence_engine.py` — structure, hygiene, rules, assembly. **Does not change.**
- `warm_lines.py` — every phrasing pool and the archetype classifier. **This is the file to
  improve.** When the better model arrives, the pools get richer; nothing else is rebuilt.

### Also tested and rejected (do not retry)

- **A second AI pass** (a `gemma4:12b` "checker" rewriting the `gemma4:26b` draft) — it
  flattened the voice into bureaucratic filler and produced every "cringe" line the owner
  rejected ("a lovely bit of news", "a standout result").
- **A free-form AI third sentence** — gemma either invented winemaking technique ("requires
  precise acid management") or restated sentence one; across six products it produced the
  same sentence five times.
- **AI-written warmth for single-award producers** — produced "It is lovely to see such
  hard work pay off like this" in 3 of 5 posts. Same cringe register.

`gemma4:26b` has a measured ceiling on warm prose. Do not expect a prompt to move it.
