# 2026-09-16 — OVERLORD Layer Provisioned (D-005) + zadocs Image Fix Underway

**Trigger:** User asked why the OVERLORD had been working alone, then confirmed the purpose was
always to oversee sub-agents: *"The concept just seem to have been ignored."*

## Root cause — the OVERLORD layer had never been built

| Check | Found | Should have been |
|---|---|---|
| `~/.hermes/profiles/` | **did not exist** | 3 role profiles |
| `hermes profile list` | only `default` | default + developer + qc-auditor + ux-designer |
| `kanban.orchestrator_profile` | `''` (empty) | `default` |
| `kanban.default_assignee` | `''` (empty) | `developer` |
| `kanban.db` task count | **0** | routed work |
| `delegation.max_iterations` | **15** | 60 |
| `delegation.child_timeout_seconds` | 600 | 1800 |

**The 15-iteration cap is the mechanical cause of the documented "14/14 subagent failure rate."**
Subagents were not incapable — they burned a 15-call budget on reconnaissance before writing a fix.
The skill's own analysis said "not incapable — dispatched wrong", yet the config was never inspected
and the failures were recorded as a permanent capability verdict.

## What was done

1. **Created the three D-003 role profiles, each on a different model family** so the verifier is not
   a clone of the implementer:
   - `developer` — `deepseek-v4-pro:0813`
   - `qc-auditor` — `kimi-k2.6`
   - `ux-designer` — `glm-5.3`
   Each smoke-tested with a literal echo check: `DEVELOPER_OK`, `QC_OK`, `UX_OK` — all passed.
2. **Set `kanban.orchestrator_profile: default`** and **`kanban.default_assignee: developer`**.
3. **Raised `delegation.max_iterations` 15 → 60** and `child_timeout_seconds` 600 → 1800.
4. **Wrote descriptions** for each profile (the kanban orchestrator uses these for routing).
5. **Proved routing end-to-end:** created card `t_1a763e96` → `hermes kanban dispatch` →
   `Spawned: 1` → worker claimed → `status: done` with result `ROUTING_PROOF_OK`. Archived after.
6. **Recorded as D-005 in `/home/m/DECISIONS.md`** — including the rule that failure is an input,
   not a verdict, and that a capability claim requires a config check first.

`hermes kanban assignees` now lists: default, developer, qc-auditor, ux-designer — all "on disk".

## The real task, now running through the OVERLORD

**Verified defect (corrected scan, 2026-09-15):** 88 articles render no article image —
**zadocs 62** of 73, **5minutes 26** of 36.

**Method proven by the OVERLORD before delegating:** `zd-embed.php` prepends the featured image as a
`<figure>` into `post_content`. Applied to zadocs post 16 → **0 → 1 visible image, confirmed on the
live rendered page** (`Johannesburg_skyline-1024x578.jpg`).

**Blocking constraint discovered:** only **28 usable topical images** exist in the zadocs media
library for 73 articles, and the featured images are heavily duplicated (media 760 used by 21
articles, 759 by 20, 758 by 20). So mapping alone cannot satisfy the 2-images-per-article rule —
each article also needs one DISTINCT image sourced from Wikimedia Commons.

That is why the work is decomposed rather than bulk-applied.

## Board state

| Card | Assignee | Purpose | Status |
|---|---|---|---|
| `t_48df4a74` | developer | PILOT: source→vision-verify→embed→verify on 5 articles (IDs 29,28,27,26,25) | running |
| `t_4344c5c1` | qc-auditor | Adversarially verify the pilot — falsify, not approve | todo (parent-gated) |
| `t_1a763e96` | qc-auditor | Routing proof | archived |

The pilot card carries the full brief in its body: site, FTP credentials, the proven script path,
the mandated `vision_analyze` question, the 2-distinct-images acceptance bar, and the rendered-page
count as the only acceptable evidence. No recon required.

The QC card is a **different model family** and is instructed to assume the developer's report is
wrong until proven otherwise, with a per-article table and explicit FAIL criteria.

**Gate:** the pilot must pass QC before any bulk rollout. Then the OVERLORD does the final visual
verification personally (D-004) — final verification is never delegated.

## Not yet done
- 5minutes 26 articles (Elementor storage — needs a different embed path than `post_content`).
- zadocs' remaining 57 articles after the pilot passes.
- beanel post 900 (1 image).
- howzitza "Low value content" review not submitted.
