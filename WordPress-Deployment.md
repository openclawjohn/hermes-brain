---
created: 2026-06-09
tags: [wordpress, deployment, workflow]
---

# WordPress Deployment Workflow

## Standard Deploy (mu-plugins)

```python
# 1. Deploy via FTP
from ftplib import FTP
ftp = FTP('cp47-jhb.za-dns.com')
ftp.login('Hermes', 'PASSWORD')
ftp.cwd('/zadocs.co.za/wp-content/mu-plugins/')
# ... upload files

# 2. Purge LiteSpeed cache
# Add PHP header after deploy:
header('X-LiteSpeed-Purge: *');
```

## Theme Editor (special case)

⚠️ **ftplib fails on special chars** — Use Python `requests` instead:

```python
import requests

# 1. Login to get cookies
session = requests.Session()
session.post('https://zadocs.co.za/wp-login.php', data={...})

# 2. Get nonce from theme editor page
nonce = extract_nonce(session.get(theme_editor_url))

# 3. POST updated code
session.post(theme_editor_url, data={
    'nonce': nonce,
    'code': updated_code
})
```

## Post-Deploy QC

1. `browser_navigate` to affected page
2. `browser_vision` to verify visually
3. Fix autonomously if wrong
4. Report only when correct

## Related
- [[ZADocs]]
- [[SumZA]]
- [[Visual-Verification-Rule]]
