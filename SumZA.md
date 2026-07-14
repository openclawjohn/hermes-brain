---
created: 2026-06-09
updated: 2026-06-09
tags: [wordpress, mu-plugin, payroll, south-africa]
host: cp47-jhb.za-dns.com
---

# SumZA

South African PAYE payroll calculator mu-plugin.

## Current Version: v18

### Features
- PAYE Gross/Net calculation with binary search
- Age field `.includes()` bug fixed
- FD[3] dropdown functionality restored

### Logo Handling
- Binarize PNG alpha channel
- Upload all sizes
- JS strips CSS background

### Deployment
- **Path:** `/wp-content/mu-plugins/` on cp47-jhb.za-dns.com
- **Method:** Python `ftplib`
- **Post-deploy:** Purge LiteSpeed cache via PHP header

### Related
- [[ZADocs]]
- [[WordPress-Deployment]]
