# ZADocs Test Site — Documentation Index

**Site:** https://test.zadocs.co.za  
**Status:** Planning Complete ✅  
**Date:** June 10, 2026

---

## Quick Start

**New to this project?** Start here:

1. **Read:** `00-QUICK-REFERENCE.md` — Credentials, commands, checklists
2. **Review:** `06-Implementation-Summary.md` — What we're building and how
3. **Begin:** Phase 1 (Foundation) — See `01-Design-Plan.md`

---

## Document Overview

### Core Planning Documents

| # | Document | Size | Purpose | When to Use |
|---|----------|------|---------|-------------|
| **00** | [QUICK-REFERENCE.md](00-QUICK-REFERENCE.md) | 6.6 KB | Credentials, commands, checklists | Daily reference |
| **01** | [Design-Plan.md](01-Design-Plan.md) | 18 KB | Complete site architecture | Before starting any phase |
| **02** | [Brand-Guidelines.md](02-Brand-Guidelines.md) | 12 KB | Logo, colors, typography | When designing pages |
| **03** | [Change-Tracker.md](03-Change-Tracker.md) | 5.4 KB | Log every change | Before/after every change |
| **04** | [Elementor-Guide.md](04-Elementor-Guide.md) | 14 KB | Step-by-step Elementor builds | When building pages |
| **05** | [Generator-Documentation.md](05-Generator-Documentation.md) | 15 KB | Document generator technical specs | Phase 5 implementation |
| **06** | [Implementation-Summary.md](06-Implementation-Summary.md) | 11 KB | 20-day plan, Git workflow | Planning sessions |
| **07** | [Site-Map-Wireframes.md](07-Site-Map-Wireframes.md) | 32 KB | Visual layouts, user flows | Design verification |

**Total:** 8 documents, ~114 KB

---

## By Task Type

### Starting Work Today

1. **Daily Workflow:** `00-QUICK-REFERENCE.md` → "Daily Workflow" section
2. **Git Commands:** `00-QUICK-REFERENCE.md` → "Git Commands" section
3. **Current Phase:** `06-Implementation-Summary.md` → "Implementation Phases"

### Designing a Page

1. **Colors/Fonts:** `02-Brand-Guidelines.md` → "Color Palette" + "Typography"
2. **Elementor Build:** `04-Elementor-Guide.md` → Specific page section
3. **Wireframe Reference:** `07-Site-Map-Wireframes.md` → Specific wireframe
4. **Log Change:** `03-Change-Tracker.md` → Add entry

### Building Generator

1. **Architecture:** `05-Generator-Documentation.md` → "System Overview"
2. **Mu-Plugin Code:** `05-Generator-Documentation.md` → "Mu-Plugin: zadocs-generator.php"
3. **Template Creation:** `05-Generator-Documentation.md` → "Template Creation Guide"
4. **Testing:** `05-Generator-Documentation.md` → "Testing Checklist"

### SEO & Content

1. **Strategy:** `01-Design-Plan.md` → "SEO Strategy"
2. **Template Content:** `01-Design-Plan.md` → "Page Templates" → "Single Template Page"
3. **FAQs:** `04-Elementor-Guide.md` → "FAQs" section
4. **Internal Linking:** `01-Design-Plan.md` → "SEO Strategy" → "Internal Linking"

### AdSense Setup

1. **Placements:** `01-Design-Plan.md` → "AdSense Optimization"
2. **Visual Map:** `07-Site-Map-Wireframes.md` → "Ad Placement Map"
3. **Styling:** `02-Brand-Guidelines.md` → "AdSense Styling"

---

## By Implementation Phase

### Phase 1: Foundation (Days 1-2)

**Primary Documents:**
- `04-Elementor-Guide.md` → "Elementor Setup Checklist"
- `02-Brand-Guidelines.md` → "Elementor Global Settings"
- `04-Elementor-Guide.md` → "Custom CSS" section

**Git Branch:** `feature/foundation`

---

### Phase 2: Homepage Complete (Days 3-4)

**Primary Documents:**
- `04-Elementor-Guide.md` → "Homepage Build Guide"
- `07-Site-Map-Wireframes.md` → "Homepage Wireframe"
- `02-Brand-Guidelines.md` → "Card Design"

**Git Branch:** `feature/homepage`

---

### Phase 3: Category Pages (Days 5-6)

**Primary Documents:**
- `04-Elementor-Guide.md` → "Category Page Structure" (modeled after single template)
- `07-Site-Map-Wireframes.md` → "Category Page Wireframe"
- `01-Design-Plan.md` → "Category Pages"

**Git Branch:** `feature/category-pages`

---

### Phase 4: Template Posts (Days 7-10)

**Primary Documents:**
- `01-Design-Plan.md` → "Template Pages"
- `04-Elementor-Guide.md` → "Single Template Page Build Guide"
- `07-Site-Map-Wireframes.md` → "Single Template Page Wireframe"

**Git Branch:** `feature/template-posts`

---

### Phase 5: Generator System (Days 11-14)

**Primary Documents:**
- `05-Generator-Documentation.md` (entire document)
- `07-Site-Map-Wireframes.md` → "Generator Page Wireframe"
- `01-Design-Plan.md` → "Document Generator System"

**Git Branch:** `feature/generator-system`

---

### Phase 6: Trust Pages (Days 15-16)

**Primary Documents:**
- `01-Design-Plan.md` → "Trust & Compliance Pages"
- `04-Elementor-Guide.md` → Standard page build pattern

**Git Branch:** `feature/trust-pages`

---

### Phase 7: SEO & AdSense (Days 17-18)

**Primary Documents:**
- `01-Design-Plan.md` → "SEO Strategy" + "AdSense Optimization"
- `07-Site-Map-Wireframes.md` → "Ad Placement Map"
- `02-Brand-Guidelines.md` → "AdSense Styling"

**Git Branch:** `feature/seo-adsense`

---

### Phase 8: Testing & Launch (Days 19-20)

**Primary Documents:**
- `06-Implementation-Summary.md` → "Launch Criteria"
- `00-QUICK-REFERENCE.md` → "QC Checklist"
- `03-Change-Tracker.md` → Verify all changes logged

**Git Branch:** `feature/launch`

---

## Quick Reference by Topic

### Credentials

**File:** `00-QUICK-REFERENCE.md` → "Credentials" section

```
WordPress: Hermes / 5xICWVAy*09urf9BSQUOT^QG
App Password: TnsQ askp jSPj TQb5 omiU iv8p
Email: hello@zadocs.co.za
```

### Colors

**File:** `02-Brand-Guidelines.md` → "Color Palette" section

```
Primary:   #0057B8
Secondary: #0099CC
Accent:    #00C896
Background: #F7F9FC
Text:      #1A1A1A
```

### Typography

**File:** `02-Brand-Guidelines.md` → "Typography" section

```
Headings: Montserrat (400, 600, 700)
Body:     Open Sans (400, 600)
```

### Git Workflow

**File:** `00-QUICK-REFERENCE.md` → "Git Commands" section

```bash
git checkout -b feature/<task>
git add .
git commit -m "verb component - reason"
git push origin feature/<task>
```

### Change Tracking

**File:** `03-Change-Tracker.md` → "Change Log Template"

**Rule:** Log EVERY change before and after

### Elementor Custom CSS

**File:** `04-Elementor-Guide.md` → "Custom CSS (Central Location)" section

**Location:** Elementor → Site Settings → Custom CSS

### Generator Shortcode

**File:** `05-Generator-Documentation.md` → "Generator Shortcode"

```
[zadocs_generator template="template-slug"]
```

### AdSense Placements

**File:** `07-Site-Map-Wireframes.md` → "Ad Placement Map"

7 placements per page (header, footer, 5 in-content)

---

## File Locations

### Obsidian (Working Docs)

```
/home/m/Documents/HermesBrain/ZADocs-Test-Site/
├── 00-QUICK-REFERENCE.md
├── 01-Design-Plan.md
├── 02-Brand-Guidelines.md
├── 03-Change-Tracker.md
├── 04-Elementor-Guide.md
├── 05-Generator-Documentation.md
├── 06-Implementation-Summary.md
└── 07-Site-Map-Wireframes.md
```

### Git Repository (Version Controlled)

```
/home/m/Sites/test-zadocs-co-za/docs/
├── 00-QUICK-REFERENCE.md
├── 01-Design-Plan.md
├── 02-Brand-Guidelines.md
├── 03-Change-Tracker.md
├── 04-Elementor-Guide.md
├── 05-Generator-Documentation.md
├── 06-Implementation-Summary.md
└── 07-Site-Map-Wireframes.md
```

**Sync Command:**
```bash
cp /home/m/Documents/HermesBrain/ZADocs-Test-Site/*.md /home/m/Sites/test-zadocs-co-za/docs/
cd /home/m/Sites/test-zadocs-co-za
git add docs/
git commit -m "Docs: update documentation"
git push origin main
```

---

## Git History

```
fd08c71 Add visual site map and wireframes
fba17fe Add quick reference card
33d6131 Initial planning documentation
```

**Total Commits:** 3  
**Total Files:** 8  
**Total Lines:** ~4,300

---

## Cross-Reference Matrix

| When I need to... | Go to document | Section |
|-------------------|----------------|---------|
| Find credentials | 00-QUICK-REFERENCE | Credentials |
| Start Git workflow | 00-QUICK-REFERENCE | Git Commands |
| Check color codes | 02-Brand-Guidelines | Color Palette |
| Build homepage | 04-Elementor-Guide | Homepage Build Guide |
| Build generator | 05-Generator-Documentation | Entire doc |
| Log a change | 03-Change-Tracker | Change Log Template |
| See wireframes | 07-Site-Map-Wireframes | Specific page |
| Check phase plan | 06-Implementation-Summary | Implementation Phases |
| Verify QC | 00-QUICK-REFERENCE | QC Checklist |
| Add AdSense | 07-Site-Map-Wireframes | Ad Placement Map |

---

## Document Update Log

| Date | Document | Change | Commit |
|------|----------|--------|--------|
| 2026-06-10 | All 8 docs | Initial creation | 33d6131, fba17fe, fd08c71 |

---

## Next Steps

1. **User Review:** Read `06-Implementation-Summary.md` for overview
2. **Approval:** Confirm plan aligns with vision
3. **Phase 1 Start:** Begin foundation work
4. **Daily:** Use `00-QUICK-REFERENCE.md` for commands/checklists

---

## Questions?

**For planning questions:** See `01-Design-Plan.md`  
**For design questions:** See `02-Brand-Guidelines.md`  
**For Elementor questions:** See `04-Elementor-Guide.md`  
**For generator questions:** See `05-Generator-Documentation.md`  
**For Git questions:** See `00-QUICK-REFERENCE.md`

---

**Remember:**
- ✅ Measure twice, cut once
- ✅ Commit before every change
- ✅ Track everything in Change Tracker
- ✅ Visual verification mandatory
- ✅ No cross-contamination with ZADocs.co.za

---

**Last Updated:** June 10, 2026  
**Status:** Planning Complete — Ready for Implementation
