# AdSense ads.txt Fix — All Portfolio Sites

**Date:** 2026-07-06
**Task:** Fix missing ads.txt files flagged by Google AdSense

## Summary

Google AdSense reported ads.txt not found for some portfolio sites. Checked all 7 sites:

| Site | ads.txt Status | Action Taken |
|------|---------------|-------------|
| beanel.com | ✅ HTTP 200 | Already present |
| howzitza.co.za | ✅ HTTP 200 | Already present |
| sumza.co.za | ✅ HTTP 200 | Already present |
| zadocs.co.za | ✅ HTTP 200 | Already present |
| **saymyname.co.za** | ❌ HTTP 404 → ✅ HTTP 200 | **Uploaded via FTP** |
| whippetqr.com | ✅ HTTP 200 | Already present |
| 5minutes.co.za | ✅ HTTP 200 | Already present |

## Fix Applied

**saymyname.co.za** was the only site missing ads.txt. Uploaded the file to the WordPress root via FTP:

```
google.com, pub-1162021827795507, DIRECT, f08c47fec0942fa0
```

**Method:** FTP upload to `cp47-jhb.za-dns.com/saymyname.co.za/ads.txt`

## Verification

All 7 sites now return HTTP 200 for `https://SITE/ads.txt` with the correct AdSense publisher ID.
