# 2026-08-04 — CEO Revamp: Chrome Connector + Reddit/Pinterest Accounts

## What Changed

The CEO was running in circles — all social platforms blocked by CAPTCHAs/Cloudflare, so it just ran maintenance checks and reported green ticks. The user pointed out this was futile.

**Solution:** User created Reddit and Pinterest accounts (signed up with hello@howzitza.co.za) logged into his Chrome. Now the CEO uses the **Chrome Connector bridge** (port 16319) to drive the user's real Chrome with cookies — no CAPTCHAs, no Cloudflare blocks.

## What Was Done

1. **Chrome Connector bridge tested** — opened Reddit in user's Chrome, loaded r/southafrica, closed the tab. Works perfectly.
2. **CEO skill updated** — Tuesday (Reddit) and Thursday (Pinterest) now use Chrome Connector bridge instructions. Added bridge startup/teardown procedure.
3. **Memory updated** — stale "all social blocked" entries replaced with current account info.
4. **Bridge startup flow added to skill** — `python3 /home/m/.hermes/scripts/chrome-bridge.py &` then wait for connection.

## How It Works Now
- CEO starts bridge → opens user's Chrome → navigates to Reddit/Pinterest (already logged in) → posts content → closes tabs
- No more "I tried and couldn't" reports
- Friday still needs LinkedIn/Medium — user may want to log into those too

## Key Decisions
- Reddit accounts created with hello@howzitza.co.za (the portfolio's general email)
- Chrome Connector bridge must be started fresh each cron run
- Tabs MUST be closed after each action (hard rule in the skill)
