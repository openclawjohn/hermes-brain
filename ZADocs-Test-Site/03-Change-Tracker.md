# ZADocs Test Site — Change Tracker

**Purpose:** Track EVERY change made to the website. No more "where did I do that?" moments.

**Rule:** If you change something, log it here IMMEDIATELY. Before and after Git commits.

---

## Session Log

### Session 1: June 10, 2026 — Planning Phase

**Goal:** Create comprehensive design plan

**Changes:**
1. Created Obsidian documentation structure
   - Path: `/home/m/Documents/HermesBrain/ZADocs-Test-Site/`
   - Files:
     - `01-Design-Plan.md` (17,972 bytes)
     - `02-Brand-Guidelines.md` (11,434 bytes)
     - `03-Change-Tracker.md` (this file)

**Git Status:**
- Repository: Not yet initialized
- Next: Initialize Git, commit baseline

**Notes:**
- User approved planning approach
- Emphasis on "measure twice, cut once"
- No cross-contamination with ZADocs.co.za

---

## Change Log Template

Use this template for each change:

```markdown
### [Date] — [What Changed]

**File:** `/path/to/file.php` (or Elementor page name)

**Before:**
- What it was
- Screenshot ref (if visual)

**After:**
- What it is now
- Screenshot ref (if visual)

**Why:**
- Reason for change

**Git Commit:**
- Branch: `feature/xxx`
- Commit: `[hash]` or "pending"
- Message: "verb what - why"

**Elementor Location:**
- Template: [name]
- Section: [number]
- Widget: [type]

**CSS Location:**
- Elementor Custom CSS: Yes/No
- Line numbers: [if applicable]

**Verification:**
- [ ] Page loads
- [ ] No console errors
- [ ] Mobile responsive
- [ ] Visual check passed
```

---

## File Inventory

### WordPress Core Files

| File | Path | Last Modified | Git Tracked |
|------|------|---------------|-------------|
| functions.php | `/wp-content/themes/hello-elementor/functions.php` | TBD | No |
| style.css | `/wp-content/themes/hello-elementor/style.css` | TBD | No |
| theme.json | `/wp-content/themes/hello-elementor/theme.json` | TBD | No |

### Mu-Plugins

| File | Path | Purpose | Last Modified |
|------|------|---------|---------------|
| zadocs-generator.php | `/wp-content/mu-plugins/zadocs-generator.php` | Document generator | TBD |
| zadocs-seo.php | `/wp-content/mu-plugins/zadocs-seo.php` | SEO enhancements | TBD |

### Elementor Templates

| Template Name | Type | ID | Last Modified | Exported |
|---------------|------|-----|---------------|----------|
| Homepage | Page | TBD | TBD | No |
| Single Document | Page | TBD | TBD | No |
| Category Archive | Archive | TBD | TBD | No |

### CSS Customizations

**Location:** Elementor → Site Settings → Custom CSS

| Section | Purpose | Lines | Last Modified |
|---------|---------|-------|---------------|
| Global Styles | Design system tokens | TBD | TBD |
| Card Hover Effects | Template cards | TBD | TBD |
| Mobile Responsiveness | Breakpoint fixes | TBD | TBD |

---

## Elementor Change Log

### Homepage Changes

| Section | Widget | Change | Date | Commit |
|---------|--------|--------|------|--------|
| Hero | Heading | H1 text set | TBD | TBD |
| Hero | Button | CTA link set | TBD | TBD |
| Categories | Grid | 6 cards created | TBD | TBD |

### Single Template Changes

| Section | Widget | Change | Date | Commit |
|---------|--------|--------|------|--------|
| Header | Heading | Dynamic title | TBD | TBD |
| Header | Button | Download link | TBD | TBD |
| Generator | HTML | Preview container | TBD | TBD |

---

## Plugin Inventory

| Plugin | Version | Status | Purpose |
|--------|---------|--------|---------|
| Elementor | TBD | Active | Page builder |
| Rank Math SEO | TBD | Active | SEO |
| WPForms Lite | TBD | Active | Contact forms |
| LiteSpeed Cache | TBD | Inactive (for now) | Caching |

---

## Known Issues

| Issue | Severity | Status | Fix Plan |
|-------|----------|--------|----------|
| None yet | N/A | N/A | N/A |

---

## Backup Log

| Date | Type | Location | Size | Verified |
|------|------|----------|------|----------|
| 2026-06-10 | Planning docs | `/home/m/Documents/HermesBrain/ZADocs-Test-Site/` | 29KB | Yes |

---

## Quick Reference

### Where to Find Things

**Elementor Settings:**
- Site Settings: Elementor hamburger menu → Site Settings
- Custom CSS: Site Settings → Custom CSS
- Global Colors: Site Settings → Global Colors
- Global Fonts: Site Settings → Global Fonts

**Theme Files:**
- Location: `/wp-content/themes/hello-elementor/`
- Edit via: FTP or WordPress Theme Editor

**Mu-Plugins:**
- Location: `/wp-content/mu-plugins/`
- Edit via: FTP only (not in WordPress admin)

**Elementor Templates:**
- Access: Templates → Saved Templates
- Export: Hover → Export

---

## Git Workflow Reminder

```bash
# Before starting work
cd /home/m/Sites/test-zadocs-co-za
git checkout main
git pull origin main
git checkout -b feature/<task-name>

# Before risky change
git add .
git commit -m "Checkpoint: <what> - safe to revert"

# After fix
git add .
git commit -m "Fix <component> - <result>"

# Daily push
git push origin feature/<task-name>
```

---

## Obsidian ↔ Git Sync

**Obsidian:** `/home/m/Documents/HermesBrain/ZADocs-Test-Site/`
**Git Repo:** `/home/m/Sites/test-zadocs-co-za/docs/`

**Sync Strategy:**
1. Write in Obsidian (rich linking, search)
2. Copy to Git repo docs/ folder (version control)
3. Commit with changes

**Command:**
```bash
cp /home/m/Documents/HermesBrain/ZADocs-Test-Site/*.md /home/m/Sites/test-zadocs-co-za/docs/
git add docs/
git commit -m "Docs: update planning documents"
```

---

**Last Updated:** June 10, 2026  
**Next Update:** After first WordPress change
