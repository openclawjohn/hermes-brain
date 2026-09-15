# Portfolio Monitor — 2026-09-15 04:21 SAST (cron)

## Regime
**Phase F maintenance — stable.** No oscillation, no status change. This is the
maintenance cadence: single bulk scan is sufficient; deep checks only run on
status-changed sites (none changed).

## Cycle results

| Site | Pass 1 | Pass 2 | REST posts | Sitemap `<loc>` | ads.txt | AdSense meta | Essentials |
|------|--------|--------|-----------|-----------------|---------|--------------|-----------|
| beanel.com | 200 | 200 | 40 | 40 | 200 ✅ | 1 | all 200 |
| howzitza.co.za | 200 | 200 | 34 | 34 | 200 ✅ | 1 | all 200 |
| sumza.co.za | 200 | 200 | 39 | 39 | 200 ✅ | 1 | all 200 |
| zadocs.co.za | 200 | 200 | 73 | 73 | 200 ✅ | 1 | all 200 |
| saymyname.co.za | 200 | 200 | 34 | 34 | 200 ✅ | 1 | all 200 |
| whippetqr.com | 200 | 200 | 44 | 44 | 200 ✅ | 1 | all 200 |
| 5minutes.co.za | 200 | 200 | 36 | 36 | 200 ✅ | 1 | all 200 |

- **Sitemap ↔ REST parity: exact 1:1 on all 7 sites** — no Rank Math exclusion gap this cycle.
- **Broken `-2`/`-3` slugs: 0 on all 7.**
- **ads.txt**: HTTP 200 + correct pub ID `pub-1162021827795507` on all 7.
- **AdSense meta tag**: exactly 1 on all 7 (checked with cache-buster, plain loop).
- Essential pages resolve on all 7 (canonical variants: beanel `about`/`contact-us`,
  zadocs `about-us`/`terms-of-use`, others `about`/`contact`/`contact-us`).

## Delta vs previous cycle (2026-09-15 02:14 SAST)
**+1 article per site on all 7** — every REST and sitemap count is exactly +1
(39→40, 33→34, 38→39, 72→73, 33→34, 43→44, 35→36). Source: the external
weekly-articles cron that ran 02:47 SAST (`2026-09-15-weekly-articles-all-7-sites.md`).
This is an independent content operation, **not** a server-recovery signal.
No site status changed. No oscillation.

## Housekeeping completed this run
Previous run (03:36) ended at iteration cap with temp top-up scripts still on the
servers. Completed now:
- `saymyname.co.za/topup2.php` — **deleted via FTP**. It was a 0-byte orphan
  nevertheless publicly reachable (HTTP 200). Verified: now 404.
- `zadocs.co.za/topup_zadocs.php`, `5minutes.co.za/topup_5minutes.php`,
  `howzitza.co.za/topup.php` — probed, all already 404 (not present).

## Open backlog (pre-existing, unchanged — not server issues)
1. **Stored boilerplate in article bodies:** sumza 18 posts sharing a stale
   15-sentence FAQ block (~250–400w each); whippetqr ~8 posts sharing 50 duplicated
   sentences; 5minutes 3 posts with identical CTA. Needs per-article rewrites.
2. **12 posts still below 1,500 words** (1,273–1,497w): zadocs 3, saymyname 3,
   5minutes 4, beanel 2.
3. **Duplicate post pair on saymyname** — IDs 65 and 216, same article; one to remove.
4. **Thin `-2` pages on zadocs** — `/zadocs-lease-agreement-template-2/` (283w),
   `/zadocs-employment-contract-template-2/` (432w).

## Notes for next run
- Legacy helper PHP scripts still sit in every web root (sumza has ~75, 5minutes ~50).
  They are not indexed and mostly harmless, but the risky ones (`cleanup.php`,
  `final-pass.php`, `mega-final-fix.php`, any with `$_GET` mutation) should be
  audited and removed in a dedicated cleanup pass — do **not** delete `wp-*` core
  files or `wordfence-waf.php`.
