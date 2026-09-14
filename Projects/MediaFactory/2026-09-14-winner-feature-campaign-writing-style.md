# 2026-09-14 — Winner-feature campaign + writing-style wiring + gateway interrupts

## Part 1 — the interrupted-session issue (why "Operation interrupted")
This session's context ballooned to ~205,000 input tokens/turn; desktop.log showed
`⚡ Interrupted during API call`. Fix: `hermes gateway restart` (auto-resumes the
session) and start a fresh chat for long work sessions. The 30-min `openclaw-heartbeat`/
`openclaw-queue` timers only watch OpenClaw crawl state files — NOT Hermes. No cron
recovers Hermes; recovery is the systemd `hermes-gateway` Restart=always + auto-resume.
Ran `hermes gateway restart` (PID 1842 → 220247) — clears stale `hermes update` warning.

## Part 2 — winner-feature campaign (commits 1ff4ee4, b4470ef, e5a0083)
Made "a campaign that features the winners" actually possible and intuitive:
- **`Campaign.participants` M2M → library.Participant** (migrations 0004/0005). A
  campaign is no longer a metadata-only shell.
- **Shared generator `content/services/winner_announcements.py::generate_winner_announcement`**
  (AI copy + overlay composite + draft ContentItem). Assets bulk-announce AND the campaign
  flow call it — one generator machine-wide.
- **Campaign detail = winner hub**: flat-row list (thumb/award/generated-status), bulk
  "Generate selected winner posts" via `common_ai_busy`, per-winner Preview → native
  `content-detail` (with Approve). Auto-features all event participants when created from
  a Winners template. Generation flash surfaced via `campaign_result`.
- **Fix for the user's AttributeError**: the `generate` view did `event.start_date`, which
  Event doesn't have. Now derives dates from template `duration_days` anchored to today.
- **Winners type + "Winner Feature" template seeded for all programmes** (Aurora already
  had "Winner Launch"). Clash "Generate Campaign" previously showed "No active templates" —
  that wall is gone. Live campaign: "Clash 2025 Winners Feature" (id 3), 35 winners featured.

## Part 3 — the writing language (SUPERSEDED — see revert note)
Initially diagnosed the voice as a "writing-style gap": winner posts were written
from generic instructions because per-event `winner-announcements-writing.md` was
never loaded. Fix `b0940d8` injected it into `build_prompt`.

**That fix was WRONG and is REVERTED (d572e13).** Injecting the thin 8-line style
files and rewriting the recipe produced verbose, stiff, "AI-sounding" posts that
matched NOTHING the user approved. Root cause of the bad voice was the
over-engineering, not a missing injection.

The approved voice is short (45-90 words), plain, warm, fact-first — exactly what
the ORIGINAL 462-line recipe + ORIGINAL prompt_builder produced (and what the 19
approved V&S posts are). Everything reverted to that state. See
`2026-09-14-winner-feature-campaign-writing-revert.md`.

## Files
- campaigns/{models,views,urls}.py, migrations 0004+0005, templates/campaigns/detail.html
- content/services/{winner_announcements.py (new), prompt_builder.py}
- Commits: 1ff4ee4, b4470ef, e5a0083, b0940d8 — all pushed to Gitea.

## Note
- Clash `gemma4:*` are stock Gemma weights (no LoRA adapter in Ollama). The "fine-tuned"
  voice is prompt/context engineering (system prompts + style files + recipe), not tuned
  weights. Flagged to user; awaiting their answer on whether a real fine-tune exists elsewhere.
