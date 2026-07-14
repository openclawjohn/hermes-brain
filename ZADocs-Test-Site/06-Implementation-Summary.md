# ZADocs Test Site — Implementation Plan Summary

**Site:** https://test.zadocs.co.za  
**Date:** June 10, 2026  
**Status:** Planning Complete — Ready for Implementation

---

## What We've Created

### Documentation (Obsidian)

**Location:** `/home/m/Documents/HermesBrain/ZADocs-Test-Site/`

1. **01-Design-Plan.md** (17,972 bytes)
   - Complete site architecture
   - 7 implementation phases
   - 101 template breakdown
   - SEO strategy
   - AdSense optimization
   - Performance targets
   - Risk mitigation

2. **02-Brand-Guidelines.md** (11,434 bytes)
   - Logo analysis and usage
   - Color palette (from brief + logo)
   - Typography (Montserrat + Open Sans)
   - Button styles
   - Card design
   - Icon system
   - Mobile-first principles
   - Accessibility guidelines

3. **03-Change-Tracker.md** (5,484 bytes)
   - Session log template
   - Change log template
   - File inventory
   - Elementor change log
   - Plugin inventory
   - Known issues tracker
   - Backup log

4. **04-Elementor-Guide.md** (14,279 bytes)
   - Step-by-step homepage build
   - Single template page build
   - Category page structure
   - Footer implementation
   - Complete Custom CSS
   - Keyboard shortcuts
   - Best practices
   - Troubleshooting

5. **05-Generator-Documentation.md** (14,971 bytes)
   - System architecture
   - Mu-plugin code
   - Generator preview template
   - Template creation guide
   - PHPWord installation
   - REST API endpoint
   - Security considerations
   - Testing checklist

### Git Repository

**Location:** `/home/m/Sites/test-zadocs-co-za/`

- ✅ Git initialized
- ✅ Main branch created
- ✅ Ready for baseline commit

---

## Implementation Phases

### Phase 1: Foundation (Days 1-2)

**Goal:** WordPress + Elementor + Design System

**Tasks:**
1. Install Hello Elementor theme
2. Install Elementor (free)
3. Install Rank Math SEO
4. Install WPForms Lite
5. Configure Elementor Site Settings
6. Set global colors/fonts
7. Add Custom CSS (design system)
8. Build homepage header
9. Build homepage hero
10. Build homepage footer

**Git Branch:** `feature/foundation`

**Deliverables:**
- Working WordPress site
- Elementor configured
- Design system active
- Homepage structure (50% complete)

---

### Phase 2: Homepage Complete (Days 3-4)

**Goal:** Full homepage with all 6 categories

**Tasks:**
1. Build categories grid (6 cards)
2. Build "How It Works" section
3. Build "Popular Downloads" section
4. Build trust indicators
5. Build SEO content block
6. Add AdSense placeholders
7. Mobile responsive testing
8. Visual verification

**Git Branch:** `feature/homepage`

**Deliverables:**
- Complete homepage
- All 6 categories linked
- Mobile responsive
- AdSense slots ready

---

### Phase 3: Category Pages (Days 5-6)

**Goal:** 6 category archive pages

**Tasks:**
1. Create category pages in WordPress:
   - Employment Documents
   - Business Documents
   - Property Documents
   - Personal Documents
   - Education Documents
   - Event Templates
2. Design category template (Elementor)
3. Add category headers
4. Add template grids
5. Add SEO content
6. Add FAQs
7. Internal linking

**Git Branch:** `feature/category-pages`

**Deliverables:**
- 6 working category pages
- Consistent design
- SEO optimized

---

### Phase 4: Template Posts (Days 7-10)

**Goal:** 101 template posts with SEO content

**Tasks:**
1. Create mu-plugin for batch post creation
2. Prepare content for 101 templates:
   - Employment: 20 templates
   - Business: 25 templates
   - Property: 15 templates
   - Personal: 20 templates
   - Education: 12 templates
   - Events: 9 templates
3. Run batch creation script
4. Add SEO content to each:
   - Overview (150 words)
   - When To Use (100 words)
   - How To Complete (200 words)
   - FAQs (10+ questions)
5. Add download buttons
6. Add related templates

**Git Branch:** `feature/template-posts`

**Deliverables:**
- 101 template posts
- SEO content complete
- Internal linking active

---

### Phase 5: Generator System (Days 11-14)

**Goal:** Working document generator

**Tasks:**
1. Install PHPWord
2. Create mu-plugin: `zadocs-generator.php`
3. Create template storage: `/wp-content/zadocs-templates/`
4. Upload 101 DOCX templates
5. Create generator preview template
6. Build generator pages (Elementor + HTML widget)
7. Implement field editing
8. Implement DOCX download
9. Implement print functionality
10. Test all 101 templates

**Git Branch:** `feature/generator-system`

**Deliverables:**
- Working generator
- All templates downloadable
- Print functionality
- Mobile responsive

---

### Phase 6: Trust Pages (Days 15-16)

**Goal:** All compliance pages

**Tasks:**
1. About Us
2. Contact Us (with WPForms)
3. Privacy Policy
4. Terms of Use
5. Disclaimer
6. Editorial Policy
7. Accessibility Statement
8. Add to footer navigation

**Git Branch:** `feature/trust-pages`

**Deliverables:**
- 7 trust pages
- Contact form working
- Footer navigation complete

---

### Phase 7: SEO & AdSense (Days 17-18)

**Goal:** SEO optimized, AdSense ready

**Tasks:**
1. Rank Math configuration
2. XML sitemap
3. Schema markup (FAQ, Article)
4. Meta titles/descriptions
5. AdSense application
6. Ad unit creation
7. Ad placement (Elementor)
8. Lazy loading
9. Performance optimization

**Git Branch:** `feature/seo-adsense`

**Deliverables:**
- SEO complete
- AdSense applied for
- Ad slots configured

---

### Phase 8: Testing & Launch (Days 19-20)

**Goal:** Production ready

**Tasks:**
1. QC Checklist (all 5 points)
2. PageSpeed optimization
3. Mobile testing (all pages)
4. Download testing (all 101 templates)
5. Form testing
6. Console error check
7. Remove "noindex"
8. Submit sitemap to Google
9. Analytics setup
10. Launch announcement

**Git Branch:** `feature/launch`

**Deliverables:**
- Production-ready site
- All tests passing
- Live and indexed

---

## Git Workflow

### Daily Workflow

```bash
# Morning: Start work
cd /home/m/Sites/test-zadocs-co-za
git checkout main
git pull origin main
git checkout -b feature/<today-task>

# During work: Commit before risky changes
git add .
git commit -m "Checkpoint: <what> - safe to revert"

# After fix: Commit result
git add .
git commit -m "Fix <component> - <result>"

# Evening: Push and merge
git push origin feature/<today-task>
# Create PR or merge directly
git checkout main
git merge feature/<today-task>
```

### Commit Message Format

```
<Verb> <Component> - <Reason>

Examples:
- "Add hero section - establishes visual hierarchy"
- "Fix category card hover - improves UX"
- "Update color palette - aligns with brand guidelines"
- "Remove unused CSS - reduces file size"
```

---

## Change Tracking

### Before Each Change

1. **Open Change Tracker:** `/home/m/Documents/HermesBrain/ZADocs-Test-Site/03-Change-Tracker.md`
2. **Log what you're about to change:**
   - File/page name
   - Before state
   - After state (planned)
   - Why
3. **Commit to Git**
4. **Make the change**
5. **Verify (browser_vision)**
6. **Update Change Tracker with actual result**
7. **Commit again**

### Example Entry

```markdown
### 2026-06-10 — Homepage Hero Background

**File:** Elementor Homepage → Hero Section

**Before:**
- White background
- No visual impact

**After:**
- Gradient background (#0057B8 → #0099CC)
- Modern fintech aesthetic

**Why:**
- Brief specifies "modern fintech platform" feel
- Gradient adds visual interest

**Git Commit:**
- Branch: `feature/homepage`
- Commit: `abc123`
- Message: "Add hero gradient - modern fintech aesthetic"

**Elementor Location:**
- Section: Hero
- Settings: Background → Gradient

**Verification:**
- [x] Page loads
- [x] No console errors
- [x] Mobile responsive
- [x] Visual check passed (browser_vision)
```

---

## Cross-Contamination Prevention

### CRITICAL: test.zadocs.co.za ≠ ZADocs.co.za

**Isolation Measures:**

1. **Separate FTP Credentials**
   - test.zadocs.co.za: Different server/login
   - ZADocs.co.za: Existing credentials
   - **NEVER mix them**

2. **Separate Git Repositories**
   - test.zadocs.co.za: `/home/m/Sites/test-zadocs-co-za/`
   - ZADocs.co.za: Existing repo
   - **NEVER commit to wrong repo**

3. **Separate Obsidian Folders**
   - test.zadocs.co.za: `/home/m/Documents/HermesBrain/ZADocs-Test-Site/`
   - ZADocs.co.za: `/home/m/Documents/HermesBrain/ZADocs/`
   - **NEVER save docs to wrong folder**

4. **Separate Branding**
   - test.zadocs.co.za: "South African Free Templates & Documents Portal"
   - ZADocs.co.za: Existing branding
   - **NEVER use wrong logo/colors**

5. **Verification Before Deploy**
   ```bash
   # Before ANY FTP upload
   pwd  # Confirm directory
   git remote -v  # Confirm repo
   # Ask: "Am I on test.zadocs or ZADocs?"
   ```

---

## Success Metrics

### Launch Criteria (All Must Pass)

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

### Immediate (User Approval Needed)

1. **Review this plan** — Does it align with your vision?
2. **Approve/modify** — Any changes to phases, design, or approach?
3. **Confirm credentials** — WordPress login working?
4. **Start Phase 1** — Begin foundation work

### After Approval

1. Copy docs to Git repo:
   ```bash
   cp /home/m/Documents/HermesBrain/ZADocs-Test-Site/*.md /home/m/Sites/test-zadocs-co-za/docs/
   cd /home/m/Sites/test-zadocs-co-za
   git add docs/
   git commit -m "Docs: Add comprehensive planning documents"
   git push origin main
   ```

2. Begin Phase 1: Foundation
   - WordPress login
   - Install theme/plugins
   - Configure Elementor
   - Build homepage structure

---

## Questions for User

Before starting implementation:

1. **Logo:** Use the "Second Logo.png" as-is, or create variations (favicon, social)?
2. **Templates:** Do you have existing DOCX templates, or should I create placeholder templates first?
3. **AdSense:** Apply immediately or wait until content is complete?
4. **Priority:** Which category should we launch with first (if not all 101 at once)?
5. **Timeline:** 20-day implementation plan acceptable, or need faster/slower?

---

**Document Status:** ✅ Planning Complete  
**Next Action:** User Review → Approval → Phase 1 Start  
**Estimated Implementation:** 20 days (full build)

---

**Remember:**
- Measure twice, cut once
- Commit before every change
- Track everything in Change Tracker
- Visual verification mandatory
- No cross-contamination with ZADocs.co.za
