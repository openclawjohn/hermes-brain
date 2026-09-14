# 2026-09-09 Effects preview white-box REAL fix + batch flow verified

## What the user reported
"Still nonsense effect after you claim you fixed it, and still no way for me to
apply an effect to all the images, and export the csv and images."

## The real bug (my earlier "crop" fix was wrong)
The Effects preview (`_render_sequentially` / `_render_all` in
app/gui/effects_view.py) was calling `prepare_cutout_fast`, which returns the
image with its white background intact. So the effect background (marble, halo)
rendered BEHIND the white box and never showed — every preview looked like the
raw photo on a white square.

My earlier "crop to content" fix only trimmed the empty border and left the
white box — it did NOT fix the real problem. The vision model confirmed: the
product sat on a white box, marble was hidden behind it.

## The real fix
The preview now calls `prepare_cutout(source, prefer_rembg=True)` — the same
rembg transparent cutout the real generation uses — so the product shows cut out
on the effect background. Verified: marble texture visible around the product
(36% non-white bg), no white box. Smoke test PASS, 9/9 pytest.

## Batch flow (verified working end-to-end)
Generate renders ALL Ready images for the chosen effect, each preview has an
"include" checkbox (default checked), and "Download selected (images + CSV)"
copies only the checked images + a filtered CSV with the FULL product data.
Verified on real data: generate 3/3, untick one, export 2 images + 2-row CSV
with content intact.

## Verification
- 9/9 pytest green
- xvfb smoke test PASS
- Real batch flow: generate 3, untick 1, export 2 + full-data CSV
- Preview render: marble visible (36% non-white bg), no white box

## Commits
- `f640265` Fix effects preview - use rembg cutout so effect background shows
