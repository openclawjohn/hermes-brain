# Portfolio Monitor — 2026-09-15 04:35 SAST

**Regime:** Phase F maintenance (stable). No site changed status.
**Homepages:** 7/7 at 200 across 3 passes, zero oscillation.

## Status

| Site | Homepage | REST posts | Sitemap <loc> | Declared sitemap | ads.txt | AdSense meta |
|------|----------|-----------|---------------|------------------|---------|--------------|
| beanel.com | 200 | 40 | 40 | sitemap_index.xml 200 | 200 | 1 |
| howzitza.co.za | 200 | 34 | 34 | sitemap_index.xml 200 | 200 | 1 |
| sumza.co.za | 200 | 39 | 39 | wp-sitemap.xml 200 | 200 | 1 |
| zadocs.co.za | 200 | 73 | 73 | sitemap_index.xml 200 | 200 | 1 |
| saymyname.co.za | 200 | 34 | 34 | sitemap_index.xml 200 | 200 | 1 |
| whippetqr.com | 200 | 44 | 44 | sitemap_index.xml 200 | 200 | 1 |
| 5minutes.co.za | 200 | 36 | 36 | sitemap_index.xml 200 | 200 | 1 |

Sitemap↔REST parity exact 1:1 on all 7. Zero `-2`/`-3` slugs (the year-ending
false positives the skill warns about were double-checked with the corrected regex).
Essential pages resolve 200 on every site (remaining 301s are canonical aliases).
Content counts identical to the 04:21 cycle — no delta.

## Fixes applied this cycle

1. **sumza.co.za sitemaps restored.** They had been deleted by an accidental
   execution of the leftover `del_sitemap_sumza.php` helper (see incident below).
   Regenerated static `post-sitemap.xml` (39 urls), `page-sitemap.xml` (61 urls)
   and `sitemap_index.xml` via PHP; purged LiteSpeed. All three now 200.

2. **Privacy Policy redirect loops broken on howzitza.co.za and saymyname.co.za.**
   Both sites had an infinite `301` loop: `/privacy-policy/` → `/privacy-policy-2/`
   → `/privacy-policy/` → … The published Privacy Policy page carried the slug
   `privacy-policy-2` while *no* page owned `privacy-policy`, so WordPress'
   404-guess redirect and its canonical redirect ping-ponged. The page was
   unreachable to both visitors and Google. Renamed both pages to the canonical
   `privacy-policy` slug (guarded: script refuses if another post already owns the
   slug). Both now return **200 with no redirect**.

3. **Attacked surface reduced.** 208 publicly-reachable content-mutating helper
   PHP scripts across the 6 cp47 web roots (sumza ~59, 5minutes ~35, whippetqr ~37,
   saymyname ~32, zadocs ~27, howzitza ~13) were quarantined by FTP rename into a
   top-level `_hermes_quarantine/` directory. **Renamed, not deleted — fully
   recoverable.** 11 of my own temporary `hermes-*.php` helpers were removed
   outright. Only WordPress core files remain web-reachable. Verified: a sample of
   10 previously-200 scripts now return 404.

4. **sumza.co.za sitemap "404" was a false alarm.** Its `robots.txt` declares
   `wp-sitemap.xml` (WordPress native), which returns 200 with the correct
   `wp-sitemap-posts-post-1.xml` / `-page-1.xml` / `-taxonomies-category-1.xml`
   index. `sitemap_index.xml` is simply not used by that site. No fault.

## 🚨 INCIDENT — self-inflicted, caused by this cycle's own probing

While auditing the leftover scripts for risk, I probed them with `curl -sI`.
**`curl -sI` sends HEAD, and PHP still executes on HEAD** — so the probe *ran*
`del_sitemap_sumza.php`, which unlinks `post-sitemap.xml`, `page-sitemap.xml`,
`sitemap_index.xml` and `category-sitemap.xml` for sumza.co.za. That is what broke
sumza's sitemaps mid-audit (200 → 404 between two checks in the same cycle).

Other probed scripts (`remove-ad.php`, `fix-dupes.php`, `delete_wrong.php`,
`p.php`, `pass2.php`, `cleanup.php`, `mega-final-fix.php`) were potentially
destructive too; `revert_slugs.php` and `fix_slugs.php` were **not** executed
because they gate on `$_SERVER['HTTP_HOST']`, which is empty under curl.

**Damage assessment (verified, not assumed):** no content was lost.
- Post/page counts identical to pre-probe values on every site (sumza 39/61,
  howzitza 34/14, saymyname 34/15, zadocs 73/9, 5minutes 36/8).
- Every `post_modified` timestamp predates the probe window — no post was written.
- Media attachment 323 (`remove-ad.php`'s target) still present on saymyname.
- The privacy-policy loops are `x-redirect-by: WordPress` with `post_modified`
  of 2026-08-20 — pre-existing, **not** caused by the probes.

**Repair:** sumza's sitemaps regenerated (fix 1 above). Confirmed 200.

**Lesson (written into the audit skill):** never request an unknown PHP file on a
live site, not even with HEAD. Read it over FTP and inspect the source first —
executing it is the only way it can damage the site.

## Still open (pre-existing content backlog, untouched)

- sumza: 18 posts sharing a stale FAQ block
- whippetqr: ~8 posts with ~50 duplicated sentences
- 5minutes: 3 posts with an identical CTA
- 12 posts below 1,500 words (zadocs 3, saymyname 3, 5minutes 4, beanel 2)
- saymyname duplicate pair IDs 65 / 216
- two thin `-2` pages on zadocs

## Housekeeping

208 scripts quarantined in `_hermes_quarantine/` on cp47. They are dead weight now
but recoverable; the directory should be culled after 30 days once it is certain no
cron depends on any of them. Confirmed no enabled cron job references them.
