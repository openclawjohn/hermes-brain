# ZADocs Test Site — Quick Reference Card

**Site:** https://test.zadocs.co.za  
**Version:** 1.0.0  
**Date:** June 10, 2026

---

## Credentials

### WordPress Admin
```
URL: https://test.zadocs.co.za/wp-admin
Username: Hermes
Password: 5xICWVAy*09urf9BSQUOT^QG
```

### Application Password (API)
```
TnsQ askp jSPj TQb5 omiU iv8p
```

### Contact Email
```
hello@zadocs.co.za
```

---

## File Locations

### Obsidian Documentation
```
/home/m/Documents/HermesBrain/ZADocs-Test-Site/
├── 01-Design-Plan.md
├── 02-Brand-Guidelines.md
├── 03-Change-Tracker.md
├── 04-Elementor-Guide.md
├── 05-Generator-Documentation.md
└── 06-Implementation-Summary.md
```

### Git Repository
```
/home/m/Sites/test-zadocs-co-za/
├── .git/
├── docs/ (planning documents)
└── CHANGELOG.md
```

### WordPress Structure
```
/wp-content/
├── themes/hello-elementor/
│   ├── functions.php
│   ├── style.css
│   └── theme.json
├── mu-plugins/
│   └── zadocs-generator.php (to be created)
├── zadocs-templates/ (to be created)
│   └── *.docx (101 templates)
└── uploads/zadocs-generated/ (temporary files)
```

---

## Brand Colors

```
Primary:   #0057B8 (ZA blue)
Secondary: #0099CC (Docs blue)
Accent:    #00C896 (Teal)
Background: #F7F9FC (Light gray-blue)
Text:      #1A1A1A (Near black)
White:     #FFFFFF
```

---

## Typography

```
Headings: Montserrat (400, 600, 700)
Body:     Open Sans (400, 600)
```

---

## Elementor Quick Access

### Site Settings
```
Elementor hamburger → Site Settings
- Global Colors
- Global Fonts
- Custom CSS (ONE location)
```

### Custom CSS Location
```
Elementor → Site Settings → Custom CSS
File: 04-Elementor-Guide.md has complete CSS
```

---

## Git Commands

### Start Work
```bash
cd /home/m/Sites/test-zadocs-co-za
git checkout main
git pull origin main
git checkout -b feature/<task-name>
```

### Commit
```bash
git add .
git commit -m "Verb component - reason"
```

### Push
```bash
git push origin feature/<task-name>
```

### Emergency Reset
```bash
git reset --hard <last-good-commit>
```

---

## Category Structure

```
Homepage
├── Employment Documents (20 templates)
├── Business Documents (25 templates)
├── Property Documents (15 templates)
├── Personal Documents (20 templates)
├── Education Documents (12 templates)
└── Event Templates (9 templates)
Total: 101 templates
```

---

## Page Templates

### Single Template Page
```
1. Header (H1, meta, download buttons)
2. AdSense (after intro)
3. Overview (what, when, how)
4. AdSense (before generator)
5. Generator Preview
6. AdSense (after generator)
7. FAQs (10+ questions)
8. Related Templates
```

### Category Page
```
1. Category Header
2. Filter Bar
3. Template Grid (3 columns)
4. SEO Content
5. FAQs
```

---

## Generator Shortcode

```
[zadocs_generator template="template-slug"]
```

**Example:**
```
[zadocs_generator template="employment-contract"]
```

---

## AdSense Placements

```
1. Header: 728x90
2. After Hero: Native ad
3. After Intro: 300x250
4. Before Generator: 728x90
5. After Generator: 300x250
6. Before FAQs: Native ad
7. Footer: 728x90
```

---

## QC Checklist (MANDATORY)

Before presenting ANY work:

- [ ] Page loads (browser_navigate)
- [ ] No console errors (browser_console)
- [ ] Visual check passed (browser_vision)
- [ ] Navigation works (test links)
- [ ] Forms submit (test contact form)
- [ ] Mobile responsive (resize test)
- [ ] Session goals achieved (review requirements)

---

## Common Elementor Paths

### Global Settings
```
Elementor → Site Settings
- Global Colors
- Global Fonts
- Custom CSS
- Layout
```

### Theme Builder (Not Available - Free Version)
```
Use: Theme header/footer + Customizer CSS
Instead of: Elementor Pro Theme Builder
```

### Saved Templates
```
Templates → Saved Templates
- Export important sections
- Import to other pages
```

---

## Mu-Plugin Development

### File Location
```
/wp-content/mu-plugins/zadocs-generator.php
```

### Testing
```php
// Add test endpoint
add_action('init', function() {
    if (!isset($_GET['test_generator'])) return;
    echo "Generator loaded";
    exit;
});
// Visit: /?test_generator=1
```

### Debugging
```php
// Log to error_log
error_log('ZADOCS: ' . print_r($data, true));
// Check: /wp-content/debug.log
```

---

## Performance Targets

```
Mobile PageSpeed:   90+
Desktop PageSpeed:  95+
Core Web Vitals:    All green
Load Time:          <2 seconds
```

---

## Security Checklist

- [ ] Nonce verification on all AJAX
- [ ] File path sanitization
- [ ] User input sanitization
- [ ] Generated file cleanup (1 hour)
- [ ] No directory traversal
- [ ] HTTPS enforced

---

## Emergency Contacts

### Site Down
```
1. Check FTP access
2. Rename suspect plugin: plugin-name → plugin-name.disabled
3. Check error logs
4. Restore from Git backup
```

### Generator Broken
```
1. Check mu-plugin syntax
2. Verify PHPWord loaded
3. Test template file exists
4. Check file permissions
```

### Can't Login
```
1. Use application password via REST API
2. Or reset via FTP (functions.php)
3. Contact hosting if locked out
```

---

## Daily Workflow

```
1. Open Obsidian docs
2. Check Change Tracker
3. Git: checkout -b feature/task
4. Make change
5. Test (browser_vision)
6. Log in Change Tracker
7. Git commit
8. Git push
9. Update session log
```

---

## Cross-Contamination Check

**BEFORE ANY FTP UPLOAD:**

```bash
# Ask yourself:
1. Am I on test.zadocs.co.za or ZADocs.co.za?
2. Is this the right FTP server?
3. Is this the right Git repo?
4. Is this the right Obsidian folder?

# Verify:
pwd  # Check directory
git remote -v  # Check repo
```

**NEVER MIX:**
- test.zadocs.co.za ≠ ZADocs.co.za
- Different FTP servers
- Different Git repos
- Different Obsidian folders

---

## Quick Links

### WordPress Admin
```
https://test.zadocs.co.za/wp-admin
```

### Elementor Editor
```
https://test.zadocs.co.za/wp-admin/edit.php?post_type=page
→ Click "Edit with Elementor"
```

### Theme Editor
```
https://test.zadocs.co.za/wp-admin/theme-editor.php
```

### Plugins
```
https://test.zadocs.co.za/wp-admin/plugins.php
```

---

## Session Goals Template

```markdown
### Session [N]: [Date] — [Goal]

**Completed:**
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

**Git Commits:**
- [hash] Task 1
- [hash] Task 2

**Change Tracker:**
- Updated: Yes/No

**Next Session:**
- [ ] Task 4
- [ ] Task 5
```

---

**Remember:**
- Measure twice, cut once
- Commit before every change
- Track everything
- Visual verification mandatory
- No cross-contamination

**Last Updated:** June 10, 2026
