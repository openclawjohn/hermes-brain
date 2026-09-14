# 2026-09-09 Double-click zoom on Effects previews

## What the user asked
"when I am in effects, that I can double click and get a much larger (full
size?) image of the particular effect. It is very difficult to see the colours
and real effect when the image is so small. So if I double click, bigger, and
when I double click again, it goes small again."

## What I built
Double-clicking any effect preview in the Effects tab opens a large 900×900
popup (CTkToplevel) showing the full-size effect render — marble texture,
colours, and product details clearly visible. Double-click the large image (or
close the window) to dismiss it. Toggle: double-click opens, double-click again
closes.

## Implementation
- Bound `<Double-Button-1>` on each preview label in `_rebuild`.
- `_toggle_zoom(slug, effect_name)`: if a zoom window is open, close it;
  otherwise render the full-size effect and open the popup.
- `_open_zoom(effect_name, full)`: creates the 900×900 popup, fits the image
  keeping aspect, binds double-click to close.
- `_close_zoom()`: destroys the popup.

## Verification
- Zoom window opens on double-click (winfo_exists = 1), closes on second
  double-click (False).
- Large render shows crisp marble texture and product details (vision-confirmed).
- 9/9 pytest green, smoke test PASS.

## Commit
`a6a2e99` Add double-click zoom on Effects previews - large popup, toggle to close
