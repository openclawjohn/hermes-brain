# ZADocs Test Site - Action Required

**Created:** June 11, 2026  
**Priority:** HIGH  
**Time Needed:** 10-15 minutes

---

## ⚡ QUICK WINS (Do These First)

### 1. Clear Elementor Cache (5 minutes)
**This fixes the homepage layout**

1. Go to: https://test.zadocs.co.za/wp-admin
2. Login: Username `Hermes`, Password `ZADocs2026!Test#`
3. In left menu: **Elementor → Tools**
4. Click: **"Clear Files & Data"** button
5. Click: **"Sync Library"** button
6. Go to: https://test.zadocs.co.za (refresh homepage)

**Expected Result:**
- Categories display in 2 rows of 3 (not stacked)
- Category cards have colored backgrounds (blue, teal, purple, orange, pink)
- Footer is blue (#0057B8) not purple
- Top navigation shows "ZADocs" + category links

---

### 2. Upload Document Generator (10 minutes)

**File 1: Mu-Plugin**
- Source: `/tmp/zadocs-generator.php`
- Destination: `/wp-content/mu-plugins/zadocs-generator.php`

**How to upload:**
```bash
# Option A: Via FTP
ftp cp47-jhb.za-dns.com
# Login: WhippetQR (see brief for password)
# Navigate to: test.zadocs.co.za/wp-content/mu-plugins/
# Upload: zadocs-generator.php

# Option B: Via WordPress File Manager plugin
# Install "WP File Manager" plugin
# Navigate to: wp-content/mu-plugins/
# Upload file

# Option C: Via SSH (if available)
scp /tmp/zadocs-generator.php user@server:/path/to/test.zadocs.co.za/wp-content/mu-plugins/
```

**File 2: Preview Template**
- Source: `/tmp/zadocs-preview-template.php`
- Destination: Your theme directory OR create a simple plugin

**Simplest approach:**
1. Create folder: `/wp-content/plugins/zadocs-preview/`
2. Create file: `zadocs-preview.php` with this content:
```php
<?php
/**
 * Plugin Name: ZADocs Preview Template
 */
// Copy contents of /tmp/zadocs-preview-template.php here
```
3. Activate plugin in WordPress

---

### 3. Test One Template (5 minutes)

After uploading the generator:

1. Go to: https://test.zadocs.co.za/employment-contract-template/
2. Click "Preview and Download" button (you'll need to add this link)
3. Or go directly to: https://test.zadocs.co.za/employment-contract-template-preview/
4. Fill in a few fields
5. Click "Download as DOCX"
6. Verify the document downloads

---

## 📋 MEDIUM PRIORITY

### 4. Link Preview Pages to Templates

Each of the 20 Employment templates needs:
- A "Preview and Download" button
- Link to its preview page

**Example for Employment Contract:**
1. Edit: Employment Contract Template post
2. Add button: "Preview and Download"
3. Link to: `/employment-contract-template-preview/`

**Repeat for all 20 templates**

---

### 5. Fix Homepage Navigation Menu

The top navigation currently has placeholder links (#). Update them:

1. **Employment** → `/category/employment-documents/`
2. **Business** → `#` (or remove until ready)
3. **Property** → `#` (or remove until ready)
4. **Personal** → `#` (or remove until ready)
5. **Education** → `#` (or remove until ready)
6. **Events** → `#` (or remove until ready)

---

## 🎯 OPTIONAL BUT RECOMMENDED

### 6. Install PHPWord (Better DOCX Generation)

If you have SSH access:
```bash
cd /path/to/test.zadocs.co.za
composer require phpoffice/phpword
```

This enables proper DOCX generation instead of HTML-based files.

---

### 7. Set Up Rank Math SEO

1. Install plugin: "Rank Math SEO"
2. Run setup wizard
3. Configure for each template page
4. Add meta descriptions
5. Set up XML sitemap

---

### 8. Configure Google AdSense

Once core functionality works:
1. Sign up for Google AdSense
2. Add verification code to site
3. Place ads in designated spots:
   - Header
   - After introduction
   - Before generator
   - After generator
   - Before FAQ
   - Footer

---

## 📞 WHAT I BUILT (Summary)

### ✅ Completed
- Homepage with full Elementor design
- 20 Employment templates with SEO content
- 5 Trust pages (About, Contact, Privacy, Terms, Disclaimer)
- 20 Preview pages (need to be linked)
- Document generator system (needs deployment)
- Complete documentation (13 files in Obsidian + Git)

### ⚠️ Needs Your Action
- Elementor cache clear (fixes layout)
- Generator upload (enables downloads)
- Preview page links (connects templates to generator)

### 📊 Site Stats
- **Pages:** 46 total
- **Templates:** 20 Employment
- **Categories:** 6 (1 active, 5 coming soon)
- **Documentation:** 13 files

---

## 🔐 CREDENTIALS QUICK REF

**WordPress Admin:**
- URL: https://test.zadocs.co.za/wp-admin
- Username: `Hermes`
- Password: `ZADocs2026!Test#`

**Application Password (for API):**
- `TnsQ askp jSPj TQb5 omiU iv8p`

**Site URL:** https://test.zadocs.co.za

---

## 📁 FILE LOCATIONS

### Generator Files (Upload These)
- `/tmp/zadocs-generator.php` → Mu-plugin
- `/tmp/zadocs-preview-template.php` → Plugin or theme

### Documentation
- Obsidian: `/home/m/Documents/HermesBrain/ZADocs-Test-Site/`
- Git: `/home/m/Sites/test-zadocs-co-za/docs/`

### Key Documents
- `11-Build-Summary.md` - Complete build overview
- `00-QUICK-REFERENCE.md` - Daily commands
- `CREDENTIALS.md` - All passwords

---

## ❓ TROUBLESHOOTING

### Homepage Still Stacked After Cache Clear?
- Hard refresh: Ctrl+F5 (Windows) or Cmd+Shift+R (Mac)
- Clear browser cache
- Check browser console for errors (F12)

### Generator Not Working?
- Check if mu-plugin is in correct folder
- Verify file permissions (644 for files, 755 for folders)
- Check WordPress debug log for errors

### Can't Upload Files?
- Use WordPress "File Manager" plugin
- Or use FTP client (FileZilla, Cyberduck)
- Contact hosting provider for FTP credentials

---

## ✅ CHECKLIST

```
[ ] 1. Clear Elementor cache
[ ] 2. Verify homepage layout (2 rows of 3 categories)
[ ] 3. Upload zadocs-generator.php to mu-plugins
[ ] 4. Upload preview template
[ ] 5. Test one template download
[ ] 6. Add "Preview and Download" buttons to all 20 templates
[ ] 7. Verify footer is blue
[ ] 8. Verify navigation links work
```

---

## 🎉 WHEN YOU'RE DONE

After completing steps 1-3, the site will have:
- ✅ Working homepage with proper layout
- ✅ Functional document generator
- ✅ Downloadable templates
- ✅ All trust pages
- ✅ Professional design

**Then I can continue with:**
- Adding remaining 81 templates
- SEO optimization
- AdSense setup
- Performance tuning

---

**Questions?** Email: tech@ttpromotions.co.za  
**Documentation:** See Obsidian vault for detailed guides
