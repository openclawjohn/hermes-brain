# 2026-09-16 — OVERLORD Layer Provisioned + zadocs 62/62 Image Fix Complete

**Trigger:** User asked why the OVERLORD was working alone, then confirmed: *"The purpose of the
overlord was to oversee sub agents, that was never in doubt from when we started this. The concept
just seem to have been ignored."* Followed by: *"as the overlord you have to make sure everything is done."*

---

## 1. Root cause — the OVERLORD layer had never been built

| Check | Found | Should be |
|---|---|---|
| `~/.hermes/profiles/` | **did not exist** | 3 role profiles |
| `hermes profile list` | only `default` | default + 3 roles |
| `kanban.orchestrator_profile` | `''` | `default` |
| `kanban.default_assignee` | `''` | `developer` |
| `kanban.db` tasks | **0** | routed work |
| `delegation.max_iterations` | **15** | 60 |
| `delegation.child_timeout_seconds` | 600 | 1800 |

**The 15-iteration cap is the mechanical cause of the recorded "14/14 subagent failure rate."**
Subagents were not incapable — they exhausted a 15-call budget on reconnaissance before writing a
fix. The skill's own analysis already said "not incapable — dispatched wrong", yet the config was
never inspected and the failures were filed as a permanent capability verdict.

## 2. The layer, built and proven

- **Three profiles on three DIFFERENT model families** (a verifier sharing the implementer's model is
  an echo, not a check): `developer` = deepseek-v4-pro:0813, `qc-auditor` = kimi-k2.6,
  `ux-designer` = glm-5.3
- Smoke-tested with literal echo checks: `DEVELOPER_OK` / `QC_OK` / `UX_OK` — all passed
- `kanban.orchestrator_profile: default`, `kanban.default_assignee: developer`
- `delegation.max_iterations` 15 → 60, `child_timeout_seconds` 600 → 1800
- **Routing proven end-to-end, not assumed:** card → `dispatch` → worker spawned → `done` with
  result `ROUTING_PROOF_OK`
- Recorded as **D-005** in `/home/m/DECISIONS.md`

## 3. Division of labour actually applied

**OVERLORD did itself** (never delegate an unproven method):
- Root-caused the defect: **Astra renders NO `<img>` for the featured image** — it appears only in
  `og:image` / `twitter:image` / JSON-LD `primaryImageOfPage`
- Proved the fix method on zadocs post 16 → 0→1 visible image, confirmed on the live rendered page
- Found the blocking constraint: only **28 topical images for 73 articles**, featured images heavily
  duplicated (media 760 ×21, 759 ×20, 758 ×20)
- **Independently verified every result** — final verification never delegated (D-004)

**Delegated: 15 cards** — 1 pilot, 12 batch cards, 1 narrow re-brief, 2 QC gates.

## 4. Result — OVERLORD-verified

| Scope | Verification |
|---|---|
| Pilot (29,28,27,26,25) | **5/5 PASS** (OVERLORD + independent qc-auditor) |
| Rollout (57 articles / 12 batches) | **57/57 PASS** (OVERLORD + qc-auditor) |
| **zadocs total** | **62/62** |

Every article: **2 distinct images, 0 empty srcs, ≥1,500 body words.**
QC auditor (different model) confirmed independently; its 3 initial apparent failures were its own
slug mismatches, resolved via the REST API and passing.

## 5. Failure handled as an INPUT, not a verdict (the point of the model)

Batch 4 exhausted its iteration budget (90/90) after 41 min. The board **auto-re-queued it to
`ready`** rather than declaring it impossible. The OVERLORD then:
1. Re-inspected the 5 articles → image 1 embedded, image 2 missing
2. Issued a **narrower card** (image 2 only; batch the downloads and the writes; target <40 iterations)
3. It completed.

No capability claim was ever made about the subagent. Only a re-brief — exactly what D-002/D-005
prescribe.

## 6. 🚨 I WAS WRONG THREE TIMES ON THE SAME MEASUREMENT

My scanners over-counted image defects on 5minutes repeatedly:

| Attempt | Method | Claim | Truth |
|---|---|---|---|
| 1 | `entry-content` class regex | 26 bad | wrong — regex matched a CSS rule |
| 2 | `<h1>`→footer, exclude `custom-logo` | 26 → then 11 | still wrong |
| 3 | minus "homepage baseline" images | 16 bad | **wrong — the homepage lists recent articles' featured images, so excluding them discarded real article images** |

**Correct method:** extract the `<article> … </article>` element, count `<img>` inside it, exclude
only the site logo. Result: **5minutes 36/36 PASS, 0 defects.**

The lesson: **a screenshot of the homepage is not site chrome.** The homepage is a list of articles
and its images ARE article images. And more generally — when three measurement attempts give three
different answers, the *method* is the bug, not the site.

## 7. Cleanup
`zd-embed.php` deleted, 404 verified. `zd-fix.php`, `zd-diag.php`, `hm_audit.php`,
`archive_diag.php` all 404.

## 8. Still open
- zadocs hero-image duplication (Sandton sunrise / Johannesburg skyline / solar heater reused as
  article 1 image). Aesthetic, not a gate failure — needs new distinct hero photos.
- beanel post 900 (1 image). howzitza "Low value content" review not yet submitted.
- 5minutes articles 132/134/136/138/140 use `_elementor_data`; their images were handled by an
  earlier cycle and verified 36/36 clean.
