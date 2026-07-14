# ZADocs Test Site — Complete Design Plan

**Site:** https://test.zadocs.co.za  
**Date:** June 10, 2026  
**Status:** Planning Phase  
**Version:** 1.0.0

---

## Executive Summary

Build a production-ready South African document template portal with 101+ free templates, document generators, AdSense optimization, and future monetization infrastructure. **Zero registration walls** — instant access, instant download.

---

## Brand Identity

### Logo Analysis

**Visual Elements:**
- **Icon:** Document/page icon with South African flag integrated into top portion
- **Flag colors:** Red, green, blue, black, yellow (Y-shape)
- **Document lines:** 3 horizontal lines representing text (dark blue, medium blue, teal)
- **Page curl:** Bottom right corner folded (white with shadow)
- **Border:** Blue outline around document

**Typography:**
- **"ZA"**: Bold, dark blue (#0057B8 matching primary color)
- **"Docs"**: Lighter blue (#0099CC matching secondary), italicized/slanted
- **Underline:** Teal swoosh (#00C896 matching accent) under "Docs"

**Brand Personality:**
- Professional yet approachable
- Clearly South African (flag integration)
- Document-focused (paper icon)
- Modern fintech aesthetic

### Color Palette (From Brief + Logo)

```
Primary:   #0057B8 (ZA blue — trust, professionalism)
Secondary: #0099CC (Docs blue — friendly, accessible)
Accent:    #00C896 (Teal swoosh — action, success)
Background: #F7F9FC (Light gray-blue — clean, modern)
Text:      #1A1A1A (Near black — readability)
White:     #FFFFFF (Cards, forms, documents)
Success:   #28a745 (Downloads, completions)
Warning:   #ffc107 (AdSense, notices)
```

---

## Technology Stack

### Confirmed Stack
- **WordPress:** Latest (via FTP access)
- **Page Builder:** Elementor (free version)
- **Theme:** Hello Elementor (lightweight, Elementor-optimized)
- **PHP:** 8+
- **Database:** MySQL
- **Cache:** LiteSpeed (to be installed post-launch)
- **CDN:** Cloudflare (to be configured)
- **SEO:** Rank Math (free)

### Critical Constraints
1. **Elementor-only editing** — no Gutenberg blocks for layouts
2. **Single CSS location** — Elementor Custom CSS only
3. **No custom scripts** when free plugins available
4. **Git version control** — commit before every change
5. **No Block Editor** — Elementor exclusively

---

## Site Architecture

### Primary Navigation

```
Home
├── Employment Documents
├── Business Documents
├── Property Documents
├── Personal Documents
├── Education Documents
└── Event Templates
```

### Secondary Navigation (Footer)

```
About Us
Contact Us
Privacy Policy
Terms of Use
Disclaimer
Editorial Policy
Accessibility Statement
```

### URL Structure

```
/ (Homepage)
/employment-documents/ (Category archive)
/employment-contract-template/ (Single template)
/employment-contract-preview/ (Generator page)
/blog/ (SEO content hub)
/about/ (Trust pages)
```

---

## Page Templates

### 1. Homepage

**Purpose:** Showcase all 6 categories, establish trust, drive AdSense impressions

**Elementor Sections:**

1. **Header**
   - Logo (left)
   - Navigation menu (center)
   - Search bar (right)
   - AdSense slot: 728x90 (below nav)

2. **Hero Section**
   - H1: "South Africa's Free Document Templates"
   - Subheading: "101+ Professional Templates — No Registration Required"
   - CTA: "Browse Templates" (anchor to categories)
   - Background: Gradient #0057B8 → #0099CC
   - AdSense slot: Native ad unit

3. **Categories Grid** (3 columns)
   - 6 category cards with icons:
     - 💼 Business Documents
     - 👥 Employment Documents
     - 🏠 Property Documents
     - 📄 Personal Documents
     - 📚 Education Documents
     - 🎉 Event Templates
   - Each card: Icon, title, description, "Free" badge
   - Hover effect: Lift + shadow

4. **Popular Downloads**
   - 6 most-downloaded templates
   - Thumbnail + title + download count
   - AdSense slot: 300x250 (sidebar)

5. **How It Works** (3 steps)
   - Step 1: Choose template
   - Step 2: Preview & customize
   - Step 3: Download instantly
   - Icons: Simple line art

6. **Trust Indicators**
   - "100% Free"
   - "No Registration"
   - "South African Compliant"
   - "Instant Download"

7. **SEO Content Block**
   - 500+ words: "Why Use South African Document Templates"
   - Internal links to categories
   - AdSense slot: In-content native

8. **Footer**
   - Logo + tagline
   - Quick links (categories)
   - Trust pages
   - Contact info
   - Copyright

---

### 2. Category Page (e.g., /employment-documents/)

**Purpose:** List all templates in category, filter/sort, AdSense

**Elementor Sections:**

1. **Category Header**
   - H1: "Employment Documents"
   - Icon + description
   - Breadcrumb navigation

2. **Filter Bar**
   - Sort by: Popular, Recent, A-Z
   - Filter by: Document type (Contract, Letter, Form)

3. **Template Grid** (3 columns)
   - Template cards with:
     - Document icon
     - Title
     - Short description
     - "Free" badge
     - Download count
     - "Preview" button

4. **SEO Content** (below fold)
   - 300+ words about category
   - FAQs (accordion)
   - Related categories

5. **AdSense Placements**
   - After header: 728x90
   - Sidebar: 300x600
   - Before FAQs: Native ad

---

### 3. Single Template Page (e.g., /employment-contract-template/)

**Purpose:** Template overview, SEO content, drive to generator

**Elementor Sections:**

1. **Template Header**
   - H1: "Employment Contract Template"
   - Icon + "Free" badge
   - Download count + last updated
   - CTA: "Preview & Download" (primary)
   - CTA: "Direct Download DOCX" (secondary)

2. **AdSense Slot** (after intro): 300x250

3. **Template Overview**
   - What is this document?
   - When to use it
   - South African legal context
   - Example snippet (visual preview)

4. **How To Complete**
   - Step-by-step guide
   - Numbered list with icons
   - Estimated completion time

5. **AdSense Slot** (before generator): 728x90

6. **Generator Preview** (embedded)
   - Live preview of document
   - Fillable fields: [Full Name], [Company], etc.
   - Real-time editing
   - Actions:
     - Download DOCX
     - Print Custom Document
     - Copy to Clipboard

7. **AdSense Slot** (after generator): 300x250

8. **FAQs** (10+ questions)
   - Accordion format
   - Schema markup for rich snippets
   - AdSense slot: Native ad before FAQs

9. **Related Templates**
   - 4-6 related documents
   - Grid layout
   - Internal linking

10. **Sponsored Block** (future-ready)
    - Hidden by default
    - Toggle via Elementor
    - Example: "Sponsored by [Payroll Software]"

---

### 4. Generator/Preview Page

**Purpose:** Interactive document customization, download

**Elementor + Custom HTML:**

1. **Document Preview Container**
   - Paper-style styling (white, shadow, A4 ratio)
   - Fillable fields (contenteditable or input overlays)
   - Real-time text replacement

2. **Control Panel** (sticky sidebar)
   - Field list (jump to section)
   - Download DOCX button
   - Print button
   - Reset button
   - Browse more templates button

3. **AdSense Slots**
   - Sidebar: 300x600
   - Below controls: 300x250

**Technical Implementation:**
- **Option A (Preferred):** Elementor HTML widget + vanilla JS
- **Option B:** Free plugin (e.g., Formidable Forms free tier)
- **DOCX Generation:** PHP library (PHPWord) via mu-plugin
- **Print:** Browser print dialog with custom CSS

---

## Document Generator System

### Architecture

```
User clicks "Preview & Download"
    ↓
Generator page loads with template HTML
    ↓
User fills in fields (real-time preview)
    ↓
User clicks "Download DOCX"
    ↓
AJAX request to mu-plugin
    ↓
PHPWord generates DOCX
    ↓
File served as download
```

### Mu-Plugin Structure

```
/wp-content/mu-plugins/zadocs-generator.php
├── Template loader (reads DOCX templates)
├── Field parser (identifies [Placeholders])
├── DOCX generator (PHPWord integration)
├── Print HTML renderer
└── Download handler
```

### Template Storage

```
/wp-content/zadocs-templates/
├── employment-contract.docx
├── lease-agreement.docx
├── invoice-template.docx
└── ... (101 templates)
```

### Field Detection Pattern

```php
// In DOCX templates, fields marked as: [Full Name], [Company], [Date]
// Regex: /\[([^\]]+)\]/
// Replaced with user input via PHPWord
```

---

## SEO Strategy

### On-Page SEO (Per Template)

1. **Template Overview** (150 words)
   - Purpose, use cases, legal context

2. **When To Use** (100 words)
   - Scenarios, examples

3. **How To Complete** (200 words)
   - Step-by-step instructions

4. **Example** (visual)
   - Completed document preview

5. **FAQs** (10+ questions)
   - Schema markup
   - Long-tail keywords

6. **Related Templates**
   - Internal linking

### Programmatic SEO

**Location Pages:**
- `/employment-contract-south-africa/`
- `/lease-agreement-south-africa/`
- `/invoice-template-south-africa/`

**Format Pages:**
- `/word-version/` (all DOCX templates)
- `/printable-version/` (all print-ready templates)

**Use-Case Pages:**
- `/small-business-invoice-template/`
- `/freelancer-invoice-template/`
- `/contractor-invoice-template/`

### Internal Linking Strategy

```
Homepage → Category → Template → Generator
     ↓          ↓          ↓
  SEO Hub   Related    Related
```

**Silo Structure:**
- Employment silo (all employment docs interlink)
- Business silo (all business docs interlink)
- Property silo (all property docs interlink)

---

## AdSense Optimization

### Placement Strategy

1. **Header:** 728x90 (below navigation)
2. **After Hero:** Native ad unit
3. **After Introduction:** 300x250 (in-content)
4. **Before Generator:** 728x90 (high visibility)
5. **After Generator:** 300x250 (post-action)
6. **Before FAQs:** Native ad
7. **Footer:** 728x90

### UX Balance

- **Above the fold:** Max 1 ad unit
- **Content-to-ad ratio:** 70/30 minimum
- **Mobile:** Sticky footer ad (320x50)
- **Load time:** Ads load after content (lazy load)

### Future Monetization

**Sponsored Blocks** (Elementor toggle):
- CV page → Recruitment platforms
- Invoice page → Accounting software
- Lease page → Property management software

**Affiliate Blocks** (central management):
- Custom Elementor widget
- Managed via WordPress admin
- Toggle on/off per page

---

## Trust & Compliance Pages

### Required Pages

1. **About Us**
   - Mission statement
   - Team (can be generic)
   - South African focus

2. **Contact Us**
   - Email: hello@zadocs.co.za
   - Contact form (free plugin: WPForms Lite)
   - Response time承诺

3. **Privacy Policy**
   - GDPR/POPIA compliant
   - No data collection (no registration)
   - AdSense disclosure

4. **Terms of Use**
   - License terms (personal/commercial use)
   - Disclaimer (not legal advice)
   - Prohibited uses

5. **Disclaimer**
   - Not legal advice
   - Templates as starting points
   - User responsibility

6. **Editorial Policy**
   - How templates are created
   - Review process
   - Update frequency

7. **Accessibility Statement**
   - WCAG 2.1 AA commitment
   - Contact for issues
   - Known limitations

---

## Performance Targets

### PageSpeed Goals

- **Mobile:** 90+
- **Desktop:** 95+
- **Core Web Vitals:** All green

### Optimization Strategy

1. **Image Optimization**
   - WebP format
   - Lazy loading
   - Max 100KB per image

2. **CSS/JS**
   - Minified
   - Combined where possible
   - Defer non-critical JS

3. **Caching** (post-launch)
   - LiteSpeed Cache plugin
   - Browser caching
   - Object cache

4. **CDN** (post-launch)
   - Cloudflare free tier
   - Static assets offloaded

---

## Content Scale Plan

### Phase 1: Launch (101 Templates)

- Employment: 20 templates
- Business: 25 templates
- Property: 15 templates
- Personal: 20 templates
- Education: 12 templates
- Events: 9 templates

### Phase 2: Growth (500+ Templates)

- Add 10 templates/week
- User requests (contact form)
- Seasonal templates (tax season, etc.)

### Phase 3: Scale (5,000+ SEO Pages)

- Programmatic SEO pages
- Location variations
- Industry variations
- Use-case variations

---

## Git Workflow

### Repository Structure

```
/home/m/Sites/test-zadocs-co-za/
├── .git/
├── docs/ (this planning folder)
├── backups/ (FTP downloads)
│   ├── theme-backup-2026-06-10/
│   ├── mu-plugins-backup-2026-06-10/
│   └── ...
└── CHANGELOG.md
```

### Commit Strategy

```bash
# Before starting work
git checkout main
git pull origin main
git checkout -b feature/<task-name>

# Before each risky change
git add .
git commit -m "Checkpoint: <what> - <why>"

# After each fix
git add .
git commit -m "Fix <component> - <result>"

# Push daily
git push origin feature/<task-name>
```

### Backup Strategy

1. **Before any code change:** Download theme files via FTP
2. **Commit backup** to Git with timestamp
3. **After major changes:** Full site backup (database + files)

---

## Obsidian Documentation Structure

```
/HermesBrain/ZADocs-Test-Site/
├── 01-Design-Plan.md (this file)
├── 02-Brand-Guidelines.md
├── 03-Elementor-Templates.md
├── 04-Generator-Documentation.md
├── 05-SEO-Strategy.md
├── 06-AdSense-Setup.md
├── 07-Git-Log.md
├── 08-Change-Tracker.md
└── Assets/
    ├── Logo/
    ├── Color-Palette.png
    └── Wireframes/
```

---

## Implementation Phases

### Phase 1: Foundation (Days 1-2)

1. **Git Setup**
   - Initialize repository
   - Create initial commit (baseline)

2. **Theme Installation**
   - Install Hello Elementor
   - Install Elementor (free)
   - Install Rank Math SEO
   - Install WPForms Lite

3. **Design System**
   - Elementor Site Settings
   - Color palette
   - Typography
   - Global buttons

4. **Homepage**
   - Hero section
   - Categories grid
   - Footer

**Git Branch:** `feature/foundation`

---

### Phase 2: Category Pages (Days 3-4)

1. **Create 6 Category Pages**
   - Employment Documents
   - Business Documents
   - Property Documents
   - Personal Documents
   - Education Documents
   - Event Templates

2. **Template Cards**
   - Design once, replicate
   - Consistent styling

3. **Navigation**
   - Menu setup
   - Breadcrumbs

**Git Branch:** `feature/category-pages`

---

### Phase 3: Template Pages (Days 5-7)

1. **Create 101 Template Posts**
   - Batch creation via mu-plugin
   - SEO content (overview, when to use, how to complete)
   - FAQs (10+ per template)

2. **Download Buttons**
   - Direct DOCX download
   - Preview & Download button

3. **Related Templates**
   - Manual linking (initially)
   - Auto-linking (future plugin)

**Git Branch:** `feature/template-pages`

---

### Phase 4: Generator System (Days 8-10)

1. **Mu-Plugin Development**
   - Template loader
   - Field parser
   - DOCX generator (PHPWord)
   - Download handler

2. **Generator Pages**
   - Elementor template
   - HTML widget for preview
   - JavaScript for field editing

3. **Testing**
   - All 101 templates
   - Download functionality
   - Print functionality

**Git Branch:** `feature/generator-system`

---

### Phase 5: Trust Pages (Days 11-12)

1. **Create All Trust Pages**
   - About Us
   - Contact Us
   - Privacy Policy
   - Terms of Use
   - Disclaimer
   - Editorial Policy
   - Accessibility Statement

2. **Contact Form**
   - WPForms setup
   - Email routing

**Git Branch:** `feature/trust-pages`

---

### Phase 6: SEO & AdSense (Days 13-14)

1. **Rank Math Configuration**
   - Sitemap
   - Schema markup
   - Meta titles/descriptions

2. **AdSense Setup**
   - Account application
   - Ad unit creation
   - Placement via Elementor

3. **SEO Content**
   - Homepage SEO block
   - Category SEO content
   - Internal linking

**Git Branch:** `feature/seo-adsense`

---

### Phase 7: Testing & Launch (Days 15-16)

1. **QC Checklist**
   - All pages load
   - Navigation works
   - Forms submit
   - Mobile responsive
   - Downloads work
   - No console errors

2. **Performance**
   - PageSpeed test
   - Image optimization
   - Cache testing

3. **Launch**
   - Remove "noindex"
   - Submit sitemap to Google
   - Analytics setup

**Git Branch:** `feature/launch-prep`

---

## Risk Mitigation

### Known Risks

1. **Elementor Free Limitations**
   - No Theme Builder
   - No global header/footer
   - **Mitigation:** Use theme header/footer + Customizer CSS

2. **Generator Complexity**
   - PHPWord learning curve
   - Template formatting issues
   - **Mitigation:** Start with 5 simple templates, test thoroughly

3. **AdSense Approval**
   - Requires quality content
   - Requires traffic
   - **Mitigation:** Focus on SEO content first, apply after 50+ templates

4. **Performance**
   - 101+ templates = large database
   - **Mitigation:** LiteSpeed Cache + Cloudflare from day 1

5. **Cross-Contamination**
   - Confusion with ZADocs.co.za
   - **Mitigation:** 
     - Different branding (test subdomain clear)
     - Separate FTP credentials
     - Git repo isolated
     - Obsidian folder isolated

---

## Success Metrics

### Launch Criteria

- [ ] 101 templates live
- [ ] All downloads working
- [ ] Generator functional
- [ ] No console errors
- [ ] Mobile PageSpeed 90+
- [ ] Desktop PageSpeed 95+
- [ ] All trust pages published
- [ ] Navigation tested
- [ ] Contact form working
- [ ] AdSense applied for

### 30-Day Goals

- 1,000+ monthly visitors
- AdSense approval received
- 500+ downloads
- 10+ user requests for new templates

### 90-Day Goals

- 10,000+ monthly visitors
- $100+ AdSense revenue
- 500+ templates
- Affiliate partnerships active

---

## Next Steps

1. **User Review:** Review this plan, approve/modify
2. **Git Setup:** Initialize repository, commit baseline
3. **Obsidian:** Create documentation structure
4. **Phase 1 Start:** Begin foundation work

---

**Document Version:** 1.0.0  
**Last Updated:** June 10, 2026  
**Next Review:** After user feedback
