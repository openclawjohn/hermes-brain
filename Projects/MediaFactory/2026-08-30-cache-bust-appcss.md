# Media Factory — Cache-Bust app.css (2026-08-30)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Problem
User reported "Status still shows no info" even after adding the `badge--queued` style. The HTML and CSS on disk were both correct (`<span class="badge badge--queued">Queued</span>` present, badge--queued rule present). Root cause: the browser was serving a **stale cached app.css** (linked with no version query), so the new badge style never loaded in the browser.

## Fix
Cache-bust the stylesheet link in `templates/base.html`: `{% static 'css/app.css' %}?v=20260830b`. Now the browser fetches a fresh `app.css`.

## Verify
- Page serves `css/app.css?v=20260830b`.
- badge--queued rule is in app.css on disk.
