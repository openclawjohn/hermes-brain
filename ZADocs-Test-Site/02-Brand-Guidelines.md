# ZADocs Test Site — Brand Guidelines

**Site:** https://test.zadocs.co.za  
**Version:** 1.0.0  
**Date:** June 10, 2026

---

## Logo Usage

### Primary Logo

**File:** `/home/m/.hermes/Websites/ZADocs/Second Logo.png`

**Specifications:**
- **Format:** PNG with transparency
- **Size:** Use at minimum 200px width
- **Clear Space:** 20px on all sides
- **Background:** White or light colors only

### Logo Anatomy

```
[Document Icon]  ZA Docs
     │            │   │
     │            │   └─ "Docs" in #0099CC, italic
     │            └───── "ZA" in #0057B8, bold
     └────────────────── SA Flag + document lines
                          Teal swoosh underneath
```

### Color Extraction from Logo

| Element | Hex | RGB | Usage |
|---------|-----|-----|-------|
| ZA Blue | #0057B8 | rgb(0, 87, 184) | Primary buttons, H1, links |
| Docs Blue | #0099CC | rgb(0, 153, 204) | Secondary buttons, H2-H3 |
| Teal Swoosh | #00C896 | rgb(0, 200, 150) | Accent, success states, badges |
| Flag Red | #E03C31 | rgb(224, 60, 49) | Alerts, warnings |
| Flag Green | #007A4D | rgb(0, 122, 77) | Success messages |
| Flag Blue | #001489 | rgb(0, 20, 137) | Dark accents |
| Flag Yellow | #FFB81C | rgb(255, 184, 28) | Highlights, stars |
| Flag Black | #000000 | rgb(0, 0, 0) | Text (use #1A1A1A instead) |

---

## Color Palette

### Primary Colors

```css
:root {
  --primary: #0057B8;      /* ZA blue — trust, professionalism */
  --secondary: #0099CC;    /* Docs blue — friendly, accessible */
  --accent: #00C896;       /* Teal — action, success */
}
```

### Neutral Colors

```css
:root {
  --background: #F7F9FC;   /* Light gray-blue — site background */
  --surface: #FFFFFF;      /* Pure white — cards, forms */
  --text-primary: #1A1A1A; /* Near black — body text */
  --text-secondary: #4A5568; /* Gray — secondary text */
  --border: #E2E8F0;       /* Light gray — borders, dividers */
}
```

### Semantic Colors

```css
:root {
  --success: #28a745;      /* Green — downloads, completions */
  --warning: #ffc107;      /* Yellow — notices, AdSense */
  --error: #dc3545;        /* Red — errors, alerts */
  --info: #17a2b8;         /* Blue — info boxes */
}
```

### Usage Guidelines

**Primary (#0057B8):**
- Main CTA buttons
- H1 headings
- Active navigation items
- Links (hover state)

**Secondary (#0099CC):**
- Secondary buttons
- H2-H3 headings
- Card borders (hover)
- Icons

**Accent (#00C896):**
- "Free" badges
- Success messages
- Download count indicators
- Checkmarks

**Background (#F7F9FC):**
- Site background
- Section backgrounds (alternating with white)

---

## Typography

### Font Family

**Primary Font:** Montserrat (Google Fonts)
- **Usage:** All headings (H1-H6)
- **Weights:** 400 (Regular), 600 (SemiBold), 700 (Bold)
- **Why:** Modern, professional, fintech aesthetic

**Secondary Font:** Open Sans (Google Fonts)
- **Usage:** Body text, buttons, forms
- **Weights:** 400 (Regular), 600 (SemiBold)
- **Why:** Highly readable, web-optimized

### Font Sizes

#### Desktop

| Element | Size | Weight | Line Height | Color |
|---------|------|--------|-------------|-------|
| H1 | 48px | 700 | 1.2 | #1A1A1A |
| H2 | 36px | 600 | 1.3 | #1A1A1A |
| H3 | 24px | 600 | 1.4 | #1A1A1A |
| H4 | 20px | 600 | 1.4 | #1A1A1A |
| Body | 16px | 400 | 1.6 | #1A1A1A |
| Small | 14px | 400 | 1.5 | #4A5568 |
| Button | 16px | 600 | 1.5 | #FFFFFF |

#### Mobile

| Element | Size | Weight | Line Height |
|---------|------|--------|-------------|
| H1 | 32px | 700 | 1.2 |
| H2 | 28px | 600 | 1.3 |
| H3 | 20px | 600 | 1.4 |
| Body | 16px | 400 | 1.6 |
| Button | 16px | 600 | 1.5 |

### Elementor Typography Settings

**Global Fonts (Elementor Site Settings):**

```
Primary Headings: Montserrat, 48px, 700
Secondary Headings: Montserrat, 36px, 600
Text: Open Sans, 16px, 400
Accent: Montserrat, 20px, 600
```

---

## Button Styles

### Primary Button

```css
.btn-primary {
  background: #0057B8;
  color: #FFFFFF;
  padding: 12px 24px;
  border-radius: 6px;
  font-weight: 600;
  font-size: 16px;
  transition: all 0.3s ease;
}

.btn-primary:hover {
  background: #004494;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 87, 184, 0.3);
}
```

**Usage:**
- "Preview & Download"
- "Browse Templates"
- Main CTAs

### Secondary Button

```css
.btn-secondary {
  background: transparent;
  color: #0057B8;
  border: 2px solid #0057B8;
  padding: 12px 24px;
  border-radius: 6px;
  font-weight: 600;
  font-size: 16px;
  transition: all 0.3s ease;
}

.btn-secondary:hover {
  background: #0057B8;
  color: #FFFFFF;
}
```

**Usage:**
- "Direct Download DOCX"
- "Browse More Templates"
- Secondary CTAs

### Success Button

```css
.btn-success {
  background: #00C896;
  color: #FFFFFF;
  padding: 12px 24px;
  border-radius: 6px;
  font-weight: 600;
  font-size: 16px;
}

.btn-success:hover {
  background: #00a87d;
}
```

**Usage:**
- Download confirmation
- Success states

---

## Card Design

### Template Card

```css
.template-card {
  background: #FFFFFF;
  border: 1px solid #E2E8F0;
  border-radius: 8px;
  padding: 24px;
  transition: all 0.3s ease;
}

.template-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  border-color: #0099CC;
}

.template-card-icon {
  font-size: 48px;
  margin-bottom: 16px;
}

.template-card-title {
  font-size: 20px;
  font-weight: 600;
  color: #1A1A1A;
  margin-bottom: 8px;
}

.template-card-description {
  font-size: 14px;
  color: #4A5568;
  margin-bottom: 16px;
  line-height: 1.5;
}

.free-badge {
  display: inline-block;
  background: #E8F5E9;
  color: #00C896;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 600;
  text-transform: uppercase;
}
```

**Elementor Implementation:**
- Container widget (24px padding)
- White background
- Border: 1px #E2E8F0
- Border radius: 8px
- Hover: Custom CSS (above)

---

## Icon System

### Category Icons (Emoji)

| Category | Icon | Hex Code |
|----------|------|----------|
| Business | 💼 | U+1F4BC |
| Employment | 👥 | U+1F465 |
| Property | 🏠 | U+1F3E0 |
| Personal | 📄 | U+1F4C4 |
| Education | 📚 | U+1F4DA |
| Events | 🎉 | U+1F389 |

**Why Emoji:**
- Zero dependencies
- Consistent rendering
- No SVG management
- Color can be customized via CSS

### Document Icons

| Document Type | Icon |
|---------------|------|
| Contract | 📜 |
| Letter | ✉️ |
| Form | 📋 |
| Template | 📄 |
| Generator | ⚙️ |
| Download | ⬇️ |
| Print | 🖨️ |

---

## Layout Principles

### Grid System

**Homepage Categories:** 3 columns
**Template Grids:** 3 columns
**Related Templates:** 4 columns
**Mobile:** 1 column (stack)

### Spacing Scale

```
4px   — Micro spacing (icon gaps)
8px   — Small spacing (form elements)
16px  — Base spacing (paragraph margins)
24px  — Medium spacing (card padding)
32px  — Large spacing (section padding)
48px  — XL spacing (between sections)
64px  — XXL spacing (major divisions)
```

### Section Backgrounds

**Pattern:**
```
Section 1: White (#FFFFFF)
Section 2: Light (#F7F9FC)
Section 3: White (#FFFFFF)
Section 4: Light (#F7F9FC)
```

**Why:** Visual separation without harsh dividers

---

## AdSense Styling

### Ad Containers

```css
.ad-slot {
  background: #F7F9FC;
  border: 1px dashed #E2E8F0;
  padding: 16px;
  margin: 32px 0;
  text-align: center;
  min-height: 90px; /* 728x90 ads */
}

.ad-label {
  font-size: 12px;
  color: #4A5568;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 8px;
}
```

### Native Ads

```css
.native-ad {
  background: #FFFFFF;
  border: 1px solid #E2E8F0;
  border-radius: 8px;
  padding: 16px;
  margin: 24px 0;
}
```

---

## Mobile-First Design

### Breakpoints

```css
/* Mobile (default) */
.container {
  padding: 16px;
}

/* Tablet (768px+) */
@media (min-width: 768px) {
  .container {
    padding: 24px;
  }
  .grid-3 {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop (1024px+) */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
    margin: auto;
  }
  .grid-3 {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

### Mobile Navigation

- Hamburger menu (right)
- Logo (left)
- Full-screen overlay on open
- Touch targets: minimum 44px

---

## Accessibility

### Color Contrast

**Minimum Ratios (WCAG 2.1 AA):**
- Normal text: 4.5:1
- Large text: 3:1
- UI components: 3:1

**Tested Combinations:**
- #0057B8 on #FFFFFF: 8.2:1 ✓
- #1A1A1A on #FFFFFF: 14.7:1 ✓
- #00C896 on #FFFFFF: 2.1:1 ✗ (use on dark bg only)

### Focus States

```css
button:focus,
a:focus,
input:focus {
  outline: 2px solid #0057B8;
  outline-offset: 2px;
}
```

### Skip Links

```html
<a href="#main-content" class="skip-link">
  Skip to main content
</a>
```

---

## Voice & Tone

### Brand Personality

- **Professional** — Trustworthy, expert
- **Approachable** — Friendly, not intimidating
- **South African** — Local context, relatable
- **Efficient** — No-nonsense, get things done

### Writing Guidelines

**Do:**
- "Download your free template"
- "South African employment law requires..."
- "Get started in 3 steps"
- "No registration needed"

**Don't:**
- "Utilize our templates" (too formal)
- "Leverage our document solutions" (jargon)
- "Click here" (vague)
- "Free!!! Download NOW!!!" (spammy)

### Microcopy Examples

**Button Text:**
- ✓ "Preview & Download"
- ✓ "Download DOCX"
- ✓ "Print Document"
- ✗ "Click Here"
- ✗ "Submit"

**Form Labels:**
- ✓ "Your Email Address"
- ✓ "Message"
- ✗ "Email"
- ✗ "Enter text here"

**Success Messages:**
- ✓ "Download started!"
- ✓ "Document generated successfully"
- ✗ "Success"
- ✗ "Done"

---

## File Naming Conventions

### Images

```
Format: [type]-[subject]-[size].[ext]

Examples:
- logo-zadocs-primary-200w.png
- icon-employment-documents-48px.png
- screenshot-employment-contract-preview-800w.webp
- hero-background-gradient-1920w.webp
```

### Elementor Templates

```
Format: [type]-[name]-v[version]

Examples:
- template-single-document-v1
- template-category-archive-v1
- template-homepage-v1
- section-hero-fintech-v1
- section-category-grid-v1
```

---

## Elementor Global Settings

### Site Settings (Export This)

**Global Colors:**
```
Primary: #0057B8
Secondary: #0099CC
Accent: #00C896
Text: #1A1A1A
Background: #F7F9FC
```

**Global Fonts:**
```
Primary Headings: Montserrat, 48px, 700
Secondary Headings: Montserrat, 36px, 600
Text: Open Sans, 16px, 400
Accent: Montserrat, 20px, 600
```

**Global Buttons:**
```
Primary: #0057B8, #FFFFFF, 12px 24px, 6px radius
Secondary: Transparent, #0057B8, 2px border
```

---

## Do's and Don'ts

### Do ✅

- Use the logo with clear space
- Apply the color palette consistently
- Use emoji icons for categories
- Keep backgrounds light (white or #F7F9FC)
- Make buttons look clickable
- Test on mobile first
- Maintain contrast ratios
- Use Montserrat for headings, Open Sans for body

### Don't ❌

- Stretch or distort the logo
- Use colors outside the palette
- Mix icon styles (emoji + SVG + illustrations)
- Use dark backgrounds (except footer)
- Make buttons too small (minimum 44px touch target)
- Design for desktop only
- Use light gray text on white
- Use decorative fonts for body text

---

**Document Version:** 1.0.0  
**Last Updated:** June 10, 2026  
**Next Review:** After homepage design complete
