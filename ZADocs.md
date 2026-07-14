---
created: 2026-06-09
tags: [wordpress, site, production]
domain: zadocs.co.za
ip: 164.160.91.40
---

# ZADocs

## Admin Access
- **Username:** Hermes
- **Password:** `Uvx@1g&70UkOQ6S#` ⚠️ **CURRENT** (updated from previous)
- **App Password:** `fO7D pYAI fHzk 7jpH MjwV 2PTh`
- **Base64 Auth:** `SGVybWVzOmZPN0QgcFlBSSBmSHprIDdqcEggTWp3ViAyUFRo`

## FTP
- **Path:** `/zadocs.co.za/` (NOT `/public_html/`)
- **Host:** cp47-jhb.za-dns.com

## Theme
- Twenty Twenty-Five (block theme)
- Custom header/footer in `functions.php`
- **Note:** Use Python `requests` for Theme Editor — `ftplib` fails on special chars

## MCP Configuration
- **Endpoints:** `/mcp-adapter-default-server`, `/elementor-mcp-server`
- **Active Plugins:** MCP Adapter, Elementor, EMCP Tools, FormLayer Pro

## Deployment Workflow
1. Python `ftplib` to `/wp-content/mu-plugins/`
2. Purge LiteSpeed cache via PHP header
3. Visual verification via `browser_vision`

## Related
- [[WordPress-Deployment]]
- [[5minutes.co.za]]
- [[SumZA]]
