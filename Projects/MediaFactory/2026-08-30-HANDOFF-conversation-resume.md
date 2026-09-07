# Media Factory — Handoff / Conversation Resume (2026-08-30)

**Purpose:** One consolidated note so a new conversation can resume instantly with full context.

## Project
- **Media Factory** — content + publishing system for the Vine & Spirit awards.
- **Server:** 192.168.7.100 · `/srv/apps/media-factory/` (Xeon/62GB/P4000).
- **Run:** `python manage.py runserver 127.0.0.1:8899` (venv).
- **Branch:** `feature/design-system-crm`, pushed to Gitea `git.myhomelab99.co.za/Media_Factory/media-factory-core.git`.
- **SSH:** `ssh m@192.168.7.100` (key id_ed25519, user 'm').

## The 3 source-of-truth files (read before work)
- `PROJECT_STATE.md` — status, what works, data, known issues.
- `DESIGN_SYSTEM.md` — tokens, components, `.gen-loading` overlay, cache-busting (`app.css?v=`).
- `RULES.md` — git + scheduling rules, `content__in` pitfall, test-data-cleanup discipline.
- Obsidian: `/home/m/Documents/HermesBrain/Projects/MediaFactory/` (many 2026-08-30 docs).

## What today accomplished (publishing/scheduling)
1. **Automatic publisher** — cron `publish_due` every minute sends `queued` jobs whose time arrived to Postiz (window-clamped 08:00–16:00, skip weekends, 3 retries). Posts publish by themselves.
2. **Publish Now / Publish All Now** — immediate send (`send_job_now`).
3. **Unified queue flow** — one Queue card; start/stop date+time, posts/day (optional), rotate, platform; drafts auto-approve on queue; default schedules ALL posts across working days.
4. **Queue detail page** — `/publishing/<slug>/<year>/queue/<id>/` lists all posts; main page Queues list is compact (count + link).
5. **Queue column + status badges** on Publishing Queue table (fixed missing `badge--queued` CSS + cache-bust).
6. **AI generation overlay** — animated, on Content Factory + Assets.
7. Many bug fixes (idempotent schedule, `content_id__in`, randomize scheduling all, removed orphaned posts, deleted test queues).

## Current live data (V&S 2026)
- **Queue: V&S Winner Roll** (id 11) — 19 winner posts, 08-31 → 08-31 08:00–16:00, 1/day, rotate, Facebook+Instagram. All 19 jobs queued for 08-31.
- Posts (approved, auto-approve on queue): Bezalel, Boschkloof, Nanola, Gravel Junction, Harmony, Joubert, Le Manoir, Long Dog, Los Locos, Skilpadvlei, Tahlia, Rocket, Nicholson Smith, +3 more. Content type `03-award-winner-announcements`.
- Admin: `admin`/`admin12345`. Postiz ids live on Programme.

## Key technical facts to remember
- `schedule_queue(queue)` is THE scheduler (all items, idempotent). `spread_queue_auto` is an alias. Avoid old `expand_rotating_queue`.
- `send_job_to_postiz(job)` = scheduled, window-clamped. `send_job_now(job)` = immediate.
- `content_id__in` for clearing jobs (never `content__in` on RotatingQueueItems).
- Cache-bust `app.css` after CSS edits; verify rendered page with curl.
- Test queues/content pollute the real DB — always clean up after validation.
