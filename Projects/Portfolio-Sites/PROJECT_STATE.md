# Portfolio Sites — Project State
## Updated: 2026-07-14

## All 7 Sites — Content Status

| Site | Posts | @1,500+ | Avg Words | Images | Status |
|------|:-----:|:-------:|:---------:|:------:|:-------|
| **sumza.co.za** | 27 | **27** ✅ | 1,619 | 27 | ✅ Complete |
| **howzitza.co.za** | 14 | **14** ✅ | 1,702 | 14 | ✅ Complete |
| **saymyname.co.za** | 31 | **31** ✅ | 1,686 | 31 | ✅ Complete |
| **5minutes.co.za** | 40 | **40** ✅ | 1,500+ | 40 | ✅ Complete (Elementor game posts verified live) |
| **whippetqr.com** | 36 | **36** ✅ | 1,805 | 36 | ✅ Complete |
| **zadocs.co.za** | 64 | **64** ✅ | 1,549 | 64 | ✅ Complete |
| **beanel.com** | 31 | **31** ✅ | 1,827 | 30 | ✅ Complete |
| **Total** | **243** | **243** | **1,677 avg** | **242** | **✅ 100%** |

## Credentials — Single Source of Truth
- `/home/m/credentials.json`
- `/home/m/SITE_CREDENTIALS.md`
- Both contain: FTP (both servers), REST API (all 7 sites), browser login, DB, AdSense

### FTP Servers
| Server | Host | User | Pass | Sites |
|--------|------|------|------|-------|
| Main (cp47) | cp47-jhb.za-dns.com (164.160.91.40) | whippetq | .tnP01u:2IZLe6 | sumza, howzitza, saymyname, 5minutes, whippetqr (public_html/), zadocs |
| Beanel | www.clashofthecultivars.com (164.160.91.56) | clashoft | w@8$VbUU$0BGC8%q}q | beanel.com (beanel.com/ dir) |

## AdSense
- Pub ID: pub-1162021827795507
- ads.txt deployed and serving on all 7 domains

## Known Issues
- 5minutes.co.za: 5 Elementor game posts (IDs 132, 134, 136, 138, 140) — content in `_elementor_data`, verified live at full 1,500+ words
- beanel.com: 1 post missing featured image (ID 900)
- All sites need ongoing content via weekly cron (Tuesdays 02:00)
