# Howzit ZA! Homepage 500 Fix + AdSense ads.txt Audit

**Date:** 2026-07-06
**Task:** Fix howzitza.co.za homepage HTTP 500 error and audit ads.txt across all portfolio sites

## Root Cause: Empty wordfence-waf.php

The howzitza.co.za homepage (and every PHP page) was returning **HTTP 500 with empty body** because the `wordfence-waf.php` file was **0 bytes (empty)**.

The `.user.ini` had:
```
auto_prepend_file = '/home/whippetq/howzitza.co.za/wordfence-waf.php'
```

This tells PHP to prepend that file to every request. An empty file caused PHP to fail silently with a 500 error. This affected:
- Homepage (500)
- All pages (500)
- REST API (500)
- Even PHP test scripts (500)

Static files (CSS, images) worked fine since they don't go through PHP.

## Fix Applied

Restored `wordfence-waf.php` with the standard Wordfence WAF loader content:
```php
<?php
if (file_exists(__DIR__ . "/wp-content/plugins/wordfence/waf/bootstrap.php")) {
    define("WFWAF_LOG_PATH", __DIR__ . "/wp-content/wflogs/");
    include_once __DIR__ . "/wp-content/plugins/wordfence/waf/bootstrap.php";
}
```

## AdSense ads.txt Audit

| Site | ads.txt | Status |
|------|---------|--------|
| beanel.com | ✅ HTTP 200 | Had file since Jun 17 |
| howzitza.co.za | ✅ HTTP 200 | Had file since Jun 25 (was unreachable due to 500) |
| sumza.co.za | ✅ HTTP 200 | Had file since Jun 19 |
| zadocs.co.za | ✅ HTTP 200 | **Was PHP-generated, now physical file** |
| saymyname.co.za | ✅ HTTP 200 | **Was missing, now uploaded** |
| whippetqr.com | ✅ HTTP 200 | Had file since Jun 18 |
| 5minutes.co.za | ✅ HTTP 200 | Had file since Jun 24 |

### Key Findings
1. **zadocs.co.za** had no physical ads.txt — WordPress was generating it virtually. Google doesn't trust PHP-generated ads.txt. Fixed by uploading physical file.
2. **saymyname.co.za** had no ads.txt at all. Fixed by uploading.
3. **howzitza.co.za** had the file but the entire site was 500ing, so Google couldn't crawl anything.
4. The other 4 sites had physical files all along — AdSense should pick them up now.

## Why AdSense Showed "Not Found" for Weeks
- **howzitza.co.za**: Site was completely down (500 error) since at least Jun 28. Google couldn't crawl anything.
- **zadocs.co.za**: ads.txt was PHP-generated, not a real static file. Google may not trust it.
- **Wordfence WAF**: Runs as PHP auto_prepend on every request. If the WAF blocks or modifies responses for Googlebot IPs, it could explain persistent failures.
- **LiteSpeed cache**: May have cached old 404 responses before the files existed.

## Verification
- Homepage: HTTP 200 ✅
- ads.txt: HTTP 200 ✅
- All pages: Working ✅
- REST API: Working ✅
- Visual: Page loads correctly with all content ✅
