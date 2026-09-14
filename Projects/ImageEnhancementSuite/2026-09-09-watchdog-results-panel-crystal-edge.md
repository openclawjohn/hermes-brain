# 2026-09-09 IES-watchdog + results-panel + Crystal Edge fixes

## Watchdog created
A watchdog cron (`IES-watchdog`, every 30m) runs the test suite + xvfb smoke
test via `~/.hermes/scripts/ies_watchdog_monitor.py`. Uses the monitor gate:
identical output = all good (agent stays asleep); changed output = a regression
or a fix, waking the agent to investigate and fix. Deterministic output
(`IES_OK` when green). `attach_to_session: true` so reports reach the user.

## Fix 1: Results panel hidden after generate
The user: "I could finally ask it to generate an effect on all images, it did it,
and then nothing. I was not asked where it should be saved, and I do not know
where it is."
Root cause: the results panel (Open the folder / Download selected / Saved in:)
was squeezed to 12px in a normal window — the new "Images to process" panel
pushed it below the visible area.
Fix: hide the selection panel when generation completes (all records are
Generated, not Ready, so it's useless) so the results panel gets the space.
Verified: results panel now 272px tall with previews + export buttons visible.

## Fix 2: Crystal Edge Separation "box within a box"
The user: "Crystal edge separation effect still has dark square that should not
be there."
Root cause: the rim_light pass drew a bright rectangle spanning the product's
aspect-fit keep-out box (864×864), masked only to remove the product's own
silhouette. Since the product rarely fills the box, the rim showed as a bright
rectangle around the box.
Fix: rim_light hugs the product's ACTUAL alpha bbox, so the rim clings to the
product's edge. Verified: bright border rectangle gone (was x=108..971, now
x=278..926 = product silhouette); background is a smooth gradient.

## Verification
- 9/9 pytest green, smoke test PASS
- Results panel 272px tall (was 12px), selection panel hidden after generate
- Crystal Edge: no bright border rectangle, smooth gradient background

## Commits
- `254c866` Fix Crystal Edge Separation box-within-a-box - rim light hugs product bbox
