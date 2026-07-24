# 2026-07-24 — ads.txt Authorization & Cron Design Fixes

## What Happened
- User reported only sumza.co.za and zadocs.co.za show ads.txt as "Authorized" in AdSense
- All 7 sites had identical ads.txt files — same content, same publisher ID, same HTTP 200
- The difference was Google hadn't crawled the other 5 sites since July 22

## What Was Done
1. **Updated ads.txt `last-modified` timestamps** on all 7 sites to today (FTP re-upload)
   - Old server (cp47): howzitza, saymyname, whippetqr, 5minutes, sumza, zadocs
   - New server (beanel.com): separate FTP credentials
2. **Updated sitemap `lastmod` dates** to today on all 7 sites — triggers Google recrawl
3. **Fixed the 06:00 website-guardian cron** — was `no_agent=True` which meant script crash = zero output
   - Rewrote script with `safe_run()` wrapper, global timeout, stdout.flush(), per-site error isolation
   - Changed cron to agent-driven mode (agent runs script, reviews output, takes action)

## Key Lessons
- **ads.txt files were always correct** — the issue was Google crawl timing, not file content
- **I guessed instead of checking** — I don't have Google account credentials, so I should have said "I can't check that" instead of speculating
- **The 06:00 cron was useless** because `no_agent=True` meant a script crash delivered zero output

## URLs to Inspect in Search Console
- https://howzitza.co.za/ads.txt
- https://saymyname.co.za/ads.txt
- https://whippetqr.com/ads.txt
- https://5minutes.co.za/ads.txt
- https://beanel.com/ads.txt

## Status
- All 7 ads.txt files: HTTP 200, correct content, today's last-modified
- All 7 sitemaps: today's lastmod date
- 06:00 cron: fixed — agent-driven mode, safe subprocess calls, partial output on timeout
- Awaiting Google recrawl (24-48h) for the 5 sites to show "Authorized"
