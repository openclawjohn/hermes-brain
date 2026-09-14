# 2026-09-09 Overlap-scan regression + CSV data-loss fixes

## What the user reported
"Now the overlap scan does not work. You are regressing. I also want to make
sure the csv has all the information like facebook atc. in the output,
otherwise I have to remarry the data which should not be necessary."

## Overlap scan regression — REVERTED
A previous "false-positive fix" (dilated the medal mask by 3px before excluding
it from the product mask) made aurora2026103 — a REAL overlap — return False.
The scan stopped flagging it. Reverted to the original undilated mask.

Verified via the real GUI flow (xvfb): scan flags aurora2026103 → Skipped,
679/6181 → Ready. This matches the documented ground truth.

**Lesson:** the vision model flip-flops on this exact image and is NOT ground
truth. The documented ground truth (PROJECT_STATE lists which images genuinely
overlap) and the user's word are. I trusted the flip-flopping vision model over
the documented ground truth and regressed the feature.

## CSV data-loss — cleaned_manifest.csv carried no product data
`clean_medal_dir` wrote only the stub (ImageID/Status/CleanedFile/Note), so the
user had to re-marry the cleaned folder's CSV to their source data.

Fix: `remove_medals_batch` now passes `records=self.records` through to
`clean_medal_dir(..., records=...)`, and the manifest carries the FULL product
data (media_url, Product, Producer, Award, Category, SubCategory, Variety,
Vintage, Notes, content, Status, CleanedFile, Note) joined by ImageID.

Verified on real data: all 3 rows carry content (FB/IG/website links).

## Stale 0-byte Assets.csv regenerated
The real exports/Assets.csv was 0 bytes from a prior run. Regenerated it via a
real batch — now 3 rows with full content. The generate Assets.csv and
download-selected CSV already carried content; the cleaned manifest was the gap.

## Verification
- 9/9 pytest, 51/51 effects render, final_audit ALL PASS, vault OK
- xvfb smoke test PASS
- Real GUI scan flow: aurora2026103 → Skipped, 679/6181 → Ready
- Real medal-removal: cleaned_manifest.csv carries full product data

## Commits
- `893a210` Revert medal-mask dilation - it broke genuine overlap detection
- `32d5e47` Carry full product data in cleaned_manifest.csv
