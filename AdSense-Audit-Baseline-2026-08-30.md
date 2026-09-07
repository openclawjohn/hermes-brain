# AdSense-Readiness Audit Baseline

Date: 2026-08-30 16:52 SAST
Type: First documented audit — no prior state in DB.

## Status: PHASE F — All 7 sites fully operational, zero oscillation (2 passes)

| Site | Home | Posts | Pages | Sitemap | REST=SM | Essential | ads.txt | AdSense Meta |
|------|------|-------|-------|---------|---------|-----------|---------|--------------|
| beanel.com | 200 | 37 | 9 | 200 | OK(37=37) | all 200 | 200 | 1 |
| howzitza.co.za | 200 | 31 | 14 | 200 | OK(31=31) | all 200 | 200 | 1 |
| sumza.co.za | 200 | 36 | 61 | 200(wp-sm) | OK(36=36) | all 200 | 200 | 1 |
| zadocs.co.za | 200 | 70 | 9 | 200 | OK(70=70) | all 200 | 200 | 1 |
| saymyname.co.za | 200 | 31 | 15 | 200 | OK(31=31) | all 200 | 200 | 1 |
| whippetqr.com | 200 | 41 | 18 | 200 | OK(41=41) | all 200 | 200 | 1 |
| 5minutes.co.za | 200 | 33 | 8 | 200 | OK(33=33) | all 200 | 200 | 1 |

- All post counts >=30 (AdSense min) ✅
- All essential pages resolve to content (301s to canonical variants OK) ✅
- ads.txt: correct pub ID on all 7 ✅
- Meta tag count=1 on all 7 (cache-busted) ✅
- No Rank Math exclusion gaps (REST == sitemap counts) ✅

## Broken slugs (-2/-3): 
- whippetqr.com: 1 (`create-qr-code-business-3-steps`)
- all others: 0

## Notes
- All homepages stable across 2 passes with 0 oscillation (maintenance mode).
- whippetqr single pre-existing broken slug — content task, not server issue.
