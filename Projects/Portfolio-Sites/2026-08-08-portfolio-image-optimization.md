# Portfolio Image Optimization (2026-08-08)

**Project:** Portfolio of 7 SA domains

## Summary
Optimized all images across the 7-site portfolio via server-side PHP recompression (GD 2.3.3, quality 82). Total saved **~282 MB** across the portfolio.

## Approach
- Audited uploads via FTP MLSD walk: ~2,900 images across 7 sites, ~1,000 MB total (beanel alone: 1,800 images, 488 MB).
- Confirmed all servers have GD 2.3.3 + WebP + 512MB memory via probe script (removed after).
- No SSH available; FTP transfer of ~1 GB too slow/risky → wrote a **server-side web-triggered PHP optimizer** (`image-opt-web.php`) deployed to each site root as `opt.php`, run in resume-safe batches via HTTP `?token&limit=N`, looping until `remaining=0`.
- JPEG recompressed at quality 82, PNG at level 9 (skips >300KB PNGs), WebP at 80.
- **Never grows a file**: only overwrites if new version is smaller (>500B). Backs up originals to `../imageopt-backup/<rel>`.

## Results
| Site | Before | After | Saved |
|------|--------|-------|-------|
| zadocs.co.za | 34.0 MB | 15.5 MB | 17.3 MB |
| sumza.co.za | 37.6 MB | 16.5 MB | 21.2 MB |
| whippetqr.com | 60.4 MB | 26.8 MB | 33.6 MB |
| howzitza.co.za | 196.9 MB | 149.1 MB | 47.9 MB |
| saymyname.co.za | 69.6 MB | 30.5 MB | 39.1 MB |
| 5minutes.co.za | 138.9 MB | 99.0 MB | 39.9 MB |
| beanel.com | 488.0 MB | 406.1 MB | 81.9 MB |
| **TOTAL** | **~1,025 MB** | **~743 MB** | **~282 MB** |

## Quality Verification
- Test image `zadocs_featured.jpg`: 640 KB → 102 KB (84% smaller), valid JPEG header.
- `vision_analyze` on optimized image: "high technical quality, no visible compression artifacts, pixelation, or banding" — professional-grade.
- All 7 sites: homepage 200, article 200, content images load (0 broken), pages render correctly.

## Files
- `image-optimizer.php` (CLI version, for SSH use) — saved to repo
- `image-opt-web.php` (web-triggered batch version used here) — saved to repo
- Deploy scripts (`opt.php` + `imageopt-state.txt`) **removed from all 7 sites** after completion.
- Backups of every changed file remain on-server in `wp-content/imageopt-backup/`.

## Next / Notes
- Originals preserved in `imageopt-backup/` if needed for rollback.
- Future images: recommend enabling LiteSpeed Image Optimization service (auto-WebP) or uploading compressed images to avoid recurrence.
