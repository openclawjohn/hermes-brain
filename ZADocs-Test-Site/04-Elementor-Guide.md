# ZADocs Test Site — Elementor Implementation Guide

**Site:** https://test.zadocs.co.za  
**Version:** 1.0.0  
**Date:** June 10, 2026

---

## Elementor Setup Checklist

### Initial Configuration

**Step 1: Install Required Plugins**

1. **Elementor** (free)
   - WordPress Admin → Plugins → Add New
   - Search: "Elementor"
   - Install → Activate

2. **Hello Elementor Theme**
   - WordPress Admin → Appearance → Themes → Add New
   - Search: "Hello Elementor"
   - Install → Activate

3. **Rank Math SEO** (free)
   - WordPress Admin → Plugins → Add New
   - Search: "Rank Math"
   - Install → Activate

4. **WPForms Lite** (free)
   - WordPress Admin → Plugins → Add New
   - Search: "WPForms"
   - Install → Activate

**Step 2: Elementor Site Settings**

Navigate to: Elementor hamburger menu → Site Settings

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

**Step 3: Load Google Fonts**

Add to Elementor Custom CSS (top of file):

```css
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&family=Open+Sans:wght@400;600&display=swap');
```

**Step 4: Set Site Layout**

Site Settings → Layout:
```
Content Width: 1200px
Stretched Section Fit To: Full Width
Entrance Animation: Fade In (optional)
```

---

## Homepage Build Guide

### Section 1: Header

**Elementor Structure:**
```
Section (1 row, 3 columns)
├── Column 1 (20% width)
│   └── Image Widget (Logo)
├── Column 2 (60% width)
│   └── Nav Menu Widget (if Pro) OR HTML Widget (manual menu)
└── Column 3 (20% width)
    └── Search Form Widget
```

**Settings:**
- Section: Sticky → Top
- Background: #FFFFFF
- Padding: 16px 0
- Border Bottom: 1px solid #E2E8F0

**Logo Widget:**
- Image: Upload Second Logo.png
- Width: 200px
- Link: Home (/)

**Menu (Manual HTML Widget):**

```html
<nav class="main-nav">
  <ul>
    <li><a href="/employment-documents/">Employment</a></li>
    <li><a href="/business-documents/">Business</a></li>
    <li><a href="/property-documents/">Property</a></li>
    <li><a href="/personal-documents/">Personal</a></li>
    <li><a href="/education-documents/">Education</a></li>
    <li><a href="/event-templates/">Events</a></li>
  </ul>
</nav>

<style>
.main-nav ul {
  display: flex;
  justify-content: center;
  gap: 32px;
  list-style: none;
  margin: 0;
  padding: 0;
}

.main-nav a {
  color: #1A1A1A;
  text-decoration: none;
  font-weight: 600;
  font-size: 16px;
  transition: color 0.3s;
}

.main-nav a:hover {
  color: #0057B8;
}
</style>
```

---

### Section 2: Hero

**Elementor Structure:**
```
Section (1 column)
└── Container (max-width: 800px)
    ├── Heading Widget (H1)
    ├── Text Editor Widget (subheading)
    └── Button Widget (CTA)
```

**Settings:**
- Section: Min Height 500px
- Background: Gradient (#0057B8 → #0099CC)
- Padding: 80px 0
- Content Position: Middle

**H1 Widget:**
```
Text: "South Africa's Free Document Templates"
Size: 48px
Weight: 700
Color: #FFFFFF
Align: Center
```

**Subheading Widget:**
```
Text: "101+ Professional Templates — No Registration Required"
Size: 20px
Weight: 400
Color: #FFFFFF (opacity 0.9)
Align: Center
Margin Top: 16px
```

**CTA Button:**
```
Text: "Browse Templates"
Link: #categories (anchor)
Size: Large
Background: #FFFFFF
Text Color: #0057B8
Border Radius: 6px
Padding: 16px 32px
Margin Top: 32px
Hover: Background #F7F9FC
```

**Custom CSS (Section):**

```css
selector {
  background: linear-gradient(135deg, #0057B8 0%, #0099CC 100%);
}

selector h1 {
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}
```

---

### Section 3: Categories Grid

**Elementor Structure:**
```
Section (ID: categories)
└── Inner Section (1 row, 3 columns)
    ├── Column 1
    │   └── Container (Category Card: Employment)
    ├── Column 2
    │   └── Container (Category Card: Business)
    ├── Column 3
    │   └── Container (Category Card: Property)
    ├── Column 4 (new row)
    │   └── Container (Category Card: Personal)
    ├── Column 5
    │   └── Container (Category Card: Education)
    └── Column 6
        └── Container (Category Card: Events)
```

**Category Card Structure (Container Widget):**

```
Container (Class: template-card)
├── Heading Widget (Icon: emoji, size 48px)
├── Heading Widget (H3: Category Name)
├── Text Editor (Description)
└── Heading Widget (Free Badge)
```

**Container Settings:**
- Background: #FFFFFF
- Border: 1px solid #E2E8F0
- Border Radius: 8px
- Padding: 24px
- Hover: Custom CSS (below)

**Custom CSS (per card):**

```css
selector.template-card {
  transition: all 0.3s ease;
}

selector.template-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  border-color: #0099CC;
}

selector .free-badge {
  display: inline-block;
  background: #E8F5E9;
  color: #00C896;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 600;
  text-transform: uppercase;
  margin-top: 12px;
}
```

**Category Data:**

| Card | Icon | Title | Description |
|------|------|-------|-------------|
| 1 | 💼 | Business Documents | Invoices, quotes, purchase orders, and business plans |
| 2 | 👥 | Employment Documents | Contracts, offer letters, warning letters, and HR forms |
| 3 | 🏠 | Property Documents | Lease agreements, rental applications, and inspection forms |
| 4 | 📄 | Personal Documents | CVs, cover letters, affidavits, and budget templates |
| 5 | 📚 | Education Documents | Study planners, homework trackers, and parent forms |
| 6 | 🎉 | Event Templates | Wedding planners, guest lists, and event checklists |

---

### Section 4: Footer

**Elementor Structure:**
```
Section (1 row, 4 columns)
├── Column 1 (25%)
│   ├── Image Widget (Logo)
│   └── Text Editor (Tagline)
├── Column 2 (25%)
│   ├── Heading Widget (Quick Links)
│   └── Icon List Widget (Category links)
├── Column 3 (25%)
│   ├── Heading Widget (Trust)
│   └── Icon List Widget (Trust pages)
└── Column 4 (25%)
    ├── Heading Widget (Contact)
    └── Text Editor (Email, info)
```

**Settings:**
- Section: Background #1A1A1A
- Padding: 64px 0
- All Text: #FFFFFF (opacity 0.9)

**Custom CSS (Footer):**

```css
selector {
  background: #1A1A1A;
  color: #FFFFFF;
}

selector h4 {
  color: #FFFFFF;
  margin-bottom: 16px;
}

selector a {
  color: rgba(255, 255, 255, 0.8);
  text-decoration: none;
  transition: color 0.3s;
}

selector a:hover {
  color: #FFFFFF;
}

selector .elementor-icon-list-icon {
  color: #00C896;
}
```

---

## Single Template Page Build Guide

### Section 1: Template Header

**Elementor Structure:**
```
Section (1 column)
└── Container (max-width: 800px)
    ├── Breadcrumbs Widget (Rank Math)
    ├── Heading Widget (H1: Template Title)
    ├── Icon List Widget (Meta: Free badge, download count)
    └── Container (Buttons: 2 buttons side-by-side)
        ├── Button (Preview & Download)
        └── Button (Direct Download DOCX)
```

**H1 Settings:**
```
Text: [Dynamic: Post Title]
Size: 40px
Weight: 700
Color: #1A1A1A
Margin: 16px 0
```

**Meta Info (Icon List):**

```
✓ Free Forever
⬇️ 1,234 Downloads
📄 Last Updated: [Dynamic: Post Modified Date]
```

**Primary Button:**
```
Text: "Preview & Download"
Link: [Custom URL: /{post-name}-preview/]
Size: Large
Background: #0057B8
Width: Full Width (mobile), Auto (desktop)
Margin: 16px 0
```

**Secondary Button:**
```
Text: "📄 Download DOCX"
Link: [Dynamic: Custom Field: docx_file_url]
Size: Medium
Style: Outline
Border Color: #0057B8
Text Color: #0057B8
```

---

### Section 2: Template Overview

**Elementor Structure:**
```
Section (1 column)
└── Container (max-width: 800px)
    ├── Heading Widget (H2: Overview)
    ├── Text Editor Widget (What is this document)
    ├── Heading Widget (H3: When To Use)
    ├── Text Editor Widget (Scenarios)
    ├── Heading Widget (H3: How To Complete)
    └── Icon List Widget (Step-by-step)
```

**Content Guidelines:**

**Overview (150 words):**
- What the document is
- Legal context (South Africa)
- Who uses it

**When To Use (100 words):**
- 3-5 common scenarios
- Practical examples

**How To Complete (200 words):**
```
1. Download the template
2. Fill in [Full Name]
3. Fill in [Company Name]
4. Fill in [Date]
5. Review and sign
6. Save or print
```

---

### Section 3: FAQs

**Elementor Structure:**
```
Section (1 column)
└── Container (max-width: 800px)
    ├── Heading Widget (H2: Frequently Asked Questions)
    └── Accordion Widget (10+ FAQs)
```

**FAQ Schema:**
- Enable Rank Math FAQ Schema
- Each accordion item = 1 FAQ
- Question = FAQ Question
- Answer = FAQ Answer

**Sample FAQs (Employment Contract):**

1. Is this employment contract legally binding in South Africa?
2. What information do I need to complete this contract?
3. Can I modify the terms in this template?
4. Do I need a lawyer to review this contract?
5. Is this contract suitable for permanent and fixed-term employment?
6. What is the notice period in South Africa?
7. Can I use this for independent contractors?
8. Do I need to register this contract with anyone?
9. What happens if either party breaches the contract?
10. Is this template compliant with the Basic Conditions of Employment Act?

---

## Custom CSS (Central Location)

**Location:** Elementor → Site Settings → Custom CSS

**Complete CSS:**

```css
/* ===================================
   ZADOCS DESIGN SYSTEM
   Location: Elementor Site Settings → Custom CSS
   Last Updated: 2026-06-10
   =================================== */

/* --- Global Styles --- */
:root {
  --primary: #0057B8;
  --secondary: #0099CC;
  --accent: #00C896;
  --background: #F7F9FC;
  --surface: #FFFFFF;
  --text: #1A1A1A;
  --border: #E2E8F0;
  --success: #28a745;
}

/* --- Typography --- */
h1, h2, h3, h4, h5, h6 {
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  line-height: 1.2;
  color: var(--text);
}

body {
  font-family: 'Open Sans', sans-serif;
  font-size: 16px;
  line-height: 1.6;
  color: var(--text);
}

/* --- Buttons --- */
.elementor-button {
  border-radius: 6px;
  font-weight: 600;
  transition: all 0.3s ease;
}

.elementor-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

/* --- Template Cards --- */
.template-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 24px;
  transition: all 0.3s ease;
}

.template-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  border-color: var(--secondary);
}

/* --- Free Badge --- */
.free-badge {
  display: inline-block;
  background: #E8F5E9;
  color: var(--accent);
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 600;
  text-transform: uppercase;
}

/* --- Ad Slots --- */
.ad-slot {
  background: var(--background);
  border: 1px dashed var(--border);
  padding: 16px;
  margin: 32px 0;
  text-align: center;
}

.ad-label {
  font-size: 12px;
  color: #666;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 8px;
}

/* --- Document Preview --- */
.doc-preview {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 40px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  max-width: 800px;
  margin: 0 auto;
}

.doc-field {
  background: #F0F4F8;
  padding: 4px 8px;
  border-radius: 4px;
  color: var(--primary);
  font-weight: 600;
}

/* --- Mobile Responsiveness --- */
@media (max-width: 767px) {
  h1 {
    font-size: 32px;
  }
  
  h2 {
    font-size: 28px;
  }
  
  .template-card {
    margin-bottom: 16px;
  }
  
  .elementor-button {
    width: 100%;
  }
}

/* --- Skip Link (Accessibility) --- */
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: var(--primary);
  color: #FFFFFF;
  padding: 8px 16px;
  z-index: 9999;
  transition: top 0.3s;
}

.skip-link:focus {
  top: 0;
}

/* --- Focus States --- */
button:focus,
a:focus,
input:focus {
  outline: 2px solid var(--primary);
  outline-offset: 2px;
}
```

---

## Elementor Keyboard Shortcuts

**General:**
- `Ctrl/Cmd + S` — Save
- `Ctrl/Cmd + Z` — Undo
- `Ctrl/Cmd + Y` — Redo
- `Ctrl/Cmd + C` — Copy
- `Ctrl/Cmd + V` — Paste

**Navigation:**
- `Esc` — Close panel / Deselect
- `Delete` — Delete selected
- `Ctrl/Cmd + Click` — Select multiple

**View:**
- `Ctrl/Cmd + Shift + L` — Library
- `Ctrl/Cmd + Shift + H` — History
- `Ctrl/Cmd + Shift + M` — Mobile preview

---

## Elementor Best Practices

### Do ✅

- Use Containers (not Sections) for new builds
- Set global colors/fonts in Site Settings
- Use Custom CSS only in Site Settings (one location)
- Name your sections/containers clearly
- Use responsive mode to test mobile
- Save templates for reuse
- Use dynamic content where possible

### Don't ❌

- Don't use Sections (deprecated — use Containers)
- Don't add CSS in individual widgets (scattered)
- Don't hardcode colors (use globals)
- Don't forget mobile testing
- Don't use too many widgets (performance)
- Don't mix Elementor with Gutenberg layouts

---

## Troubleshooting

### Issue: Changes Don't Show

**Solution:**
1. Clear Elementor cache: Elementor → Tools → Regenerate CSS
2. Clear browser cache: Ctrl+Shift+R
3. Add `?nocache=1` to URL

### Issue: Mobile Layout Broken

**Solution:**
1. Switch to Mobile View (bottom panel)
2. Check each section's mobile settings
3. Adjust padding/margins for mobile
4. Use Custom CSS media queries

### Issue: Global Colors Not Applying

**Solution:**
1. Check if widget has manual color set
2. Remove manual color (use default/global)
3. Regenerate CSS in Elementor Tools

---

**Document Version:** 1.0.0  
**Last Updated:** June 10, 2026  
**Next Review:** After homepage build complete
