# IES Watchdog — Xvfb xkbdir smoke-test fix

**Date:** 2026-09-10
**Branch:** `fix/ies-watchdog-xkbdir` (in `~/.hermes/scripts`)

## What happened
The IES-watchdog cron fired `IES_FAIL tests=True smoke=False` — the smoke
test could not connect to the X display.

## Root cause (NOT an IES code regression)
The `xkb-data` package is in a broken `iU` (unpacked-not-configured) dpkg
state, leaving `/usr/share/X11/xkb/` empty (only a `.dpkg-staging-dir`).
`xvfb-run`'s Xvfb then failed to compile the keymap and died before the
display came up:

```
XKB: Failed to compile keymap
Keyboard initialization failed.
Fatal server error: Failed to activate virtual core keyboard: 2
_tkinter.TclError: couldn't connect to display ":109"
```

A full copy of the xkb data exists at `/usr/share/X11/xkb.dpkg-backup/`.

## Fix
`~/.hermes/scripts/ies_watchdog_monitor.py` now starts Xvfb manually with
`-xkbdir /usr/share/X11/xkb.dpkg-backup` (falling back to the default dir
when it's intact) instead of relying on `xvfb-run -a`. This is a monitor
infrastructure fix, not an IES app change.

## Verification
- Unit tests: `pytest tests/test_engine.py -q` → **9 passed**
- Smoke test: `DISPLAY=:99 python tests/overlord_smoke.py` → **SMOKE TEST PASS** (11 previews rendered)
- Monitor: `python3 ies_watchdog_monitor.py` → **IES_OK**

## Still open
The underlying `xkb-data` dpkg breakage needs a root fix
(`dpkg --configure xkb-data` or reinstall). The monitor workaround is
harmless and keeps the watchdog green meanwhile. No sudo available on this
box, so the root fix is a manual/ops step.

## Resolution (2026-09-10, watchdog re-fire)
The watchdog re-fired because the monitor output CHANGED — this time to
`IES_OK` (a fix, not a regression). Confirmed the underlying `xkb-data`
breakage has since been configured: `/usr/share/X11/xkb/` is populated and
`/usr/share/X11/xkb.dpkg-backup/` no longer exists. The monitor's `-xkbdir`
fallback is now inert (default dir present) but harmless. Re-verified on the
re-fire: unit tests **9 passed**, smoke test **SMOKE TEST PASS** (11 previews),
monitor emits **IES_OK**. No IES code change was needed this session — only a
PROJECT_STATE doc update (commit `0575815`, merged to main).
