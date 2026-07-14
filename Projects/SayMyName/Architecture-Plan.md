# SayMyName.co.za — Comprehensive Architecture & Planning Document

> **Domain:** saymyname.co.za
> **Platform:** WordPress + Astra Child Theme
> **Monetization:** 100% AdSense (no paywalls, no email capture)
> **Date:** June 2026
> **Status:** Pre-development — Planning Complete

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Market Analysis & Competitive Landscape](#2-market-analysis--competitive-landscape)
3. [South African Naming Conventions Research](#3-south-african-naming-conventions-research)
4. [Enhanced Feature Set](#4-enhanced-feature-set)
5. [Technical Architecture](#5-technical-architecture)
6. [SEO & Content Strategy](#6-seo--content-strategy)
7. [AdSense Monetization Strategy](#7-adsense-monetization-strategy)
8. [File-by-File Implementation Plan](#8-file-by-file-implementation-plan)
9. [Phased Development Roadmap](#9-phased-development-roadmap)
10. [Name Dataset Strategy](#10-name-dataset-strategy)
11. [Performance Targets](#11-performance-targets)
12. [Risk Assessment & Mitigation](#12-risk-assessment--mitigation)

---

## 1. Executive Summary

SayMyName.co.za will be South Africa's first dedicated multi-category name generator with deep support for all 11 official South African language groups. The site is 100% free, monetized entirely through Google AdSense, and built for maximum performance on WordPress with an Astra Child theme.

**Key Differentiators:**
- **First SA-focused name generator** — No competitor targets South African naming specifically
- **11 Official languages** — Zulu, Xhosa, Afrikaans, English, Sepedi, Setswana, Sesotho, Xitsonga, Siswati, Tshivenda, isiNdebele
- **Culturally authentic** — Names generated using real linguistic patterns, not random syllables
- **100% free, zero friction** — No signup, no email, no paywalls. Pure ad-supported model
- **WhatsApp-native sharing** — Critical for SA market where WhatsApp dominates social sharing
- **Programmatic SEO** — Hundreds of indexable pages targeting long-tail SA name queries

**Revenue Model:** AdSense with 3 manual placements + Auto Ads. Target $5-15 RPM. Break-even at ~30K monthly visits.

---

## 2. Market Analysis & Competitive Landscape

### 2.1 Competitor Analysis Summary

| Site | Strengths | Weaknesses | Gap for SayMyName |
|------|-----------|------------|-------------------|
| **Namelix.com** | AI-powered, clean UX, domain check | Business only, no SA focus, upsells to paid | SA languages, 100% free |
| **BusinessNameGenerator.com** | 18 languages, industry filters, content marketing | Content-heavy, slow, no SA focus | SA-specific, faster UX |
| **Nameberry.com** | Name meanings, trending, mobile app, community | Baby only, premium tier, US-centric | SA languages, free |
| **BehindTheName.com** | Deep etymology, 30+ countries, authoritative | No copy buttons, no save, old UX | Modern UX, SA focus |
| **Shopify BNG** | Clean, fast, no signup | E-commerce only, Shopify funnel | Multi-category, SA focus |
| **NameStation.com** | AI Autopilot, trademark check, team workspaces | Paid model, no SA focus | 100% free, SA languages |
| **NameChef.com** | Swipe matching, partner mode | Baby only, premium | Multi-category, free |
| **FantasyNameGenerators.com** | Massive category variety (fantasy, RPG) | No real-world names, no SA focus | Real SA names |

### 2.2 Market Gap Analysis

**The Big Gap:** There is NO South African name generator. No site offers:
- Zulu, Xhosa, Afrikaans, Sotho, Tswana name generation
- .co.za domain availability checking
- South African business naming conventions
- Township/small business naming trends
- Pronunciation guides for African names
- WhatsApp sharing of generated names

**Secondary Gaps:**
- Most competitors have premium tiers — a truly free, ad-supported model is rare
- Most competitors don't offer export (CSV/PDF)
- Most competitors don't offer batch generation (50+ names at once)
- Most competitors don't offer name meanings alongside generation

### 2.3 Traffic & Revenue Projections

| Milestone | Monthly Visits | Est. RPM | Monthly Revenue |
|-----------|---------------|----------|-----------------|
| Launch (Month 1-3) | 1K-5K | $3-5 | $3-25 |
| Growth (Month 4-6) | 5K-20K | $5-8 | $25-160 |
| Steady State (Month 7-12) | 20K-100K | $8-15 | $160-1,500 |
| Mature (Year 2+) | 100K-500K | $10-20 | $1,000-10,000 |

---

## 3. South African Naming Conventions Research

### 3.1 Language Groups & Name Patterns

#### Nguni Languages (Zulu, Xhosa, isiNdebele, Siswati)

**Common Characteristics:**
- Vowel-heavy, often ending in -a, -e, -i, -o
- Click consonants in Xhosa (c, q, x) — distinctive feature
- Feminine prefixes: No- (mother of), Noma- (who is with), Non- (with)
- Masculine prefixes: Si- (we), Nko- (king/chief), Nku- (freedom)
- Common suffixes: -le (full/complete), -ile (has done), -ani (doer), -o (abstract)

**Zulu Name Examples:**
- Male: Sibusiso, Bongani, Dumisani, Jabulani, Sifiso, Siyabonga, Thabo, Themba, Vusumuzi, Xolani, Nkosana, Nkululeko, Sandile, Zwelakhe
- Female: Sibongile, Thandiwe, Thandi, Lindiwe, Zanele, Zodwa, Zethu, Busisiwe, Duduzile, Jabulile, Khanyisile, Nokwethemba, Nonhlanhla, Nonkululeko

**Xhosa Name Examples:**
- Male: Andile, Anele, Ayanda, Bheki, Sipho, Thando, Siyabonga, Themba, Xolani, Sihle
- Female: Nomsa, Noxolo, Nontando, Zintle, Busisiwe, Nomalanga, Nomathemba, Nombulelo, Nomvula, Zanele

#### Sotho-Tswana Languages (Sesotho, Sepedi, Setswana)

**Common Characteristics:**
- Often start with L-, K-, T-, M-, R-
- Common endings: -o, -e, -a
- Feminine: often end in -o (Lerato, Palesa, Thato)
- Masculine: often end in -o (Thabo, Karabo, Katlego)
- Prefixes: Mo- (person), Ba- (people group)

**Name Examples:**
- Male: Thabo, Karabo, Katlego, Lebogang, Ofentse, Gomolemo, Tseko, Tshegofatso, Tshiamo
- Female: Lerato, Palesa, Mmabatho, Dipuo, Rethabile, Nthabiseng, Thato, Lesedi, Lesego

#### Afrikaans

**Traditional/Old Afrikaans (Dutch-derived):**
- Male: Pieter, Johannes, Jacobus, Hendrik, Willem, Andries, Francois, Stephanus, Marthinus, Christiaan
- Female: Maria, Johanna, Anna, Catharina, Susanna, Elizabeth, Magdalena, Martha, Cornelia, Hester
- Surnames: Van der Merwe, Botha, Pretorius, Du Plessis, De Klerk, Van Wyk, Venter, Fourie, Coetzee, Nel, Grobler, Van der Walt, Du Toit, Erasmus, Steyn, Malan, De Villiers, Joubert, Kruger, Meyer
- Patterns: "Van der/den" (of the), "De" (the), patronymic suffixes like "-se" (son of)

**Modern Afrikaans:**
- Male: Liam, Ethan, Noah, Ryan, Kyle, Chad, Juan, Ruan, Keegan, Devon
- Female: Charné, Chané, Mieke, Suné, Anke, Bianca, Tania, Rene, Sanet, Elize
- Patterns: Accented é endings, shorter forms, English/American influence

#### English South African

- Follows British/Commonwealth patterns with SA-specific variations
- Traditional: John, David, Michael, Robert, William, James, Charles, George, Henry, Thomas
- Female: Elizabeth, Margaret, Catherine, Anne, Mary, Sarah, Jane, Helen, Susan, Patricia
- Modern SA: Similar to UK/Australian trends with nature names and unique spellings

### 3.2 Business Naming in SA

- **Afrikaans businesses:** "Die" (The), "& Seuns" (& Sons), "Handelaars" (Traders)
- **English businesses:** Standard international patterns
- **African-language businesses:** Cultural concepts, proverbs
- **Common prefixes:** "Afri-", "SA-", "RSA-", "Mzansi-"
- **Trend:** Bilingual/bicultural names, acronyms, founder surnames

### 3.3 Pet Naming in SA

- **Dogs:** Max, Bella, Charlie, Kosie, Snoekie, Thandi, Simba
- **Cats:** Similar patterns, more whimsical
- **Trend:** Human names for pets, food names, pop culture references
- **Afrikaans pet names:** Diminutives ending in -ie (Snoekie, Kosie, Flippie)

### 3.4 Data Sources for Name Lists

| Source | Type | Quality | Notes |
|--------|------|---------|-------|
| Wikipedia SA name categories | Web | Medium | 54+ names per category, limited |
| BehindTheName.com | Web | High | Has Zulu, Xhosa, Afrikaans categories |
| StatsSA birth data | Government | High | Name popularity data (not public API) |
| Academic papers | Research | High | Search "South African naming traditions" |
| Nameberry SA lists | Web | Medium | Curated lists, US-centric |
| SA History Online | Web | Medium | Many 404s, but good when available |

---

## 4. Enhanced Feature Set

### 4.1 Phase 1 — Core Features (MVP)

| Feature | Complexity | User Value | Differentiator? | Implementation |
|---------|-----------|-----------|----------------|----------------|
| **Multi-category generation** | Low | Critical | No | Category selector: Business, Baby, Pet, Product, Software, Character |
| **Style/Modifier filters** | Low | Critical | No | Modern, Vintage, British, Afrikaans Old, Afrikaans Modern, Xhosa, Zulu, Sotho, Sepedi, Setswana, Tsonga, Venda, Ndebele, Swati |
| **One-click copy** | Low | Critical | No | Clipboard API with visual feedback |
| **Batch generation (10 names)** | Low | High | Yes | Show 10 names at once, "Generate More" appends |
| **Generate More (infinite)** | Low | High | Yes | Append to results, no page reload |
| **AdSense placeholder divs** | Low | Required | No | #smn-ad-above-fold, #smn-ad-in-feed, #smn-ad-below-results |
| **Server-rendered first batch** | Medium | High | No | SEO-critical — names in initial HTML |
| **Clean URL structure** | Medium | High | No | /generator/{category}/{style}/ |
| **Dynamic SEO meta** | Medium | High | No | Rank Math hooks for title/description per combo |
| **Static SEO content blocks** | Medium | High | Yes | Pre-written intro, tips, FAQs per category+style |
| **Responsive design** | Low | Critical | No | Mobile-first, works on all devices |

### 4.2 Phase 2 — Enhanced Features (Weeks 2-4)

| Feature | Complexity | User Value | Differentiator? | Implementation |
|---------|-----------|-----------|----------------|----------------|
| **Name meaning display** | Low | High | Yes | Show meaning alongside each generated name |
| **Favorites (localStorage)** | Low | High | Moderate | Heart icon, saved to localStorage, viewable list |
| **WhatsApp sharing** | Low | Very High | **YES** | wa.me link with pre-filled name text |
| **Gender filter (baby names)** | Low | High | No | Boy/Girl/Unisex toggle for baby category |
| **Name length filter** | Low | Medium | No | Short/Medium/Long slider or buttons |
| **Starting letter filter** | Low | Medium | No | A-Z letter picker |
| **Dark mode toggle** | Low | Medium | No | CSS custom properties, localStorage preference |
| **Name history** | Low | Medium | Moderate | Last 20 generated names in session |
| **Export as TXT** | Low | Medium | Yes | Download generated names as text file |
| **Pronunciation guide** | Medium | Very High | **YES** | Phonetic spelling for African names (e.g., "si-BOO-see-oh" for Sibusiso) |

### 4.3 Phase 3 — Advanced Features (Month 2+)

| Feature | Complexity | User Value | Differentiator? | Implementation |
|---------|-----------|-----------|----------------|----------------|
| **Multi-language UI** | Medium | High | **YES** | English, Afrikaans, isiZulu UI toggle |
| **Export as CSV** | Medium | Medium | Yes | Download as spreadsheet-compatible CSV |
| **Similar name suggestions** | Medium | High | Moderate | "More like this" button per name |
| **Shareable permalinks** | Medium | Medium | Moderate | URL that reproduces a set of names |
| **Name popularity indicators** | Medium | Medium | No | Show popularity rank/trend if data available |
| **Batch generation (50 names)** | Low | High | Yes | "Generate 50" button for power users |
| **.co.za domain checking** | High | High | **YES** | API call to check domain availability |
| **Name comparison** | Medium | Medium | Moderate | Side-by-side name comparison |
| **Keyboard shortcuts** | Low | Low | No | G=Generate, C=Copy, F=Favorite |

### 4.4 Phase 4 — Long-Term Moats (Month 3+)

| Feature | Complexity | User Value | Differentiator? | Implementation |
|---------|-----------|-----------|----------------|----------------|
| **Pronunciation audio** | High | Very High | **YES** | Recorded audio for African names (hard to replicate) |
| **Community features** | High | High | Yes | Name ratings, comments, user-submitted names |
| **Mobile app (PWA)** | Medium | High | Yes | Progressive Web App with offline support |
| **SA domain registrar partnership** | High | High | **YES** | Affiliate integration with .co.za registrars |
| **AI-powered name suggestions** | High | High | Yes | LLM-based name generation for unique requests |

---

## 5. Technical Architecture

### 5.1 Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                    Browser (Client)                      │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ smn-core.js  │  │ smn-generator│  │ Category JSON │  │
│  │ (2KB, sync)  │  │ .js (15KB,   │  │ (lazy-loaded  │  │
│  │              │  │  dynamic     │  │  by category) │  │
│  │ UI handlers  │  │  import())   │  │               │  │
│  │ State mgmt  │  │ Generation   │  │ 30-80KB each │  │
│  │ Ad placehold│  │ algorithms   │  │ gzip-compress │  │
│  └──────┬──────┘  └──────┬───────┘  └───────┬───────┘  │
│         │                │                  │           │
│         └────────────────┴──────────────────┘           │
│                         │                               │
│                         ▼                               │
│              ┌─────────────────────┐                     │
│              │  WordPress REST API │                     │
│              │  /wp-json/saymyname │                     │
│              │  /v1/generate       │                     │
│              └─────────┬───────────┘                     │
└────────────────────────┼────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│                    Server (WordPress)                    │
│  ┌──────────────────────────────────────────────────┐  │
│  │              smn-generator-core Plugin              │  │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────────────┐  │  │
│  │  │ REST     │ │ Dataset  │ │ Server Generator │  │  │
│  │  │ Controller│ │ Loader   │ │ (PHP fallback)   │  │  │
│  │  └──────────┘ └──────────┘ └──────────────────┘  │  │
│  └──────────────────────────────────────────────────┘  │
│                         │                               │
│                         ▼                               │
│  ┌──────────────────────────────────────────────────┐  │
│  │              Caching Layer                         │  │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────────────┐  │  │
│  │  │ Transient│ │ Page     │ │ Object Cache     │  │  │
│  │  │ API      │ │ Cache    │ │ (Redis if avail) │  │  │
│  │  └──────────┘ └──────────┘ └──────────────────┘  │  │
│  └──────────────────────────────────────────────────┘  │
│                         │                               │
│                         ▼                               │
│  ┌──────────────────────────────────────────────────┐  │
│  │         Astra Child Theme (page-generator.php)    │  │
│  │  - Full-width layout                              │  │
│  │  - Enqueues plugin assets                          │  │
│  │  - Dynamic SEO content blocks                      │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### 5.2 Key Architectural Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| **Generation approach** | Hybrid: Server-first batch, then client-side | SEO requires names in HTML; UX requires instant subsequent generations |
| **API method** | WordPress REST API | Cacheable, modern, works with caching plugins |
| **Data format** | Flat JSON arrays per category | Fastest JS access, gzip-compresses well |
| **Client storage** | sessionStorage + localStorage | sessionStorage for session state, localStorage for preferences |
| **Code splitting** | Dynamic import() for generator engine | Core JS is 2KB, generator is 15KB lazy-loaded |
| **CSS approach** | Custom .smn-* classes, no framework | Theme-agnostic, minimal, no dependencies |
| **Plugin structure** | No DB queries, no jQuery dependency | Maximum performance, zero database load |
| **URL structure** | Clean URLs via WordPress rewrite rules | SEO-friendly, indexable per combination |
| **Progressive enhancement** | Form-based fallback with noscript | Works without JS at basic level |

### 5.3 Repository Structure

```
saymyname-project/
├── .gitignore
├── README.md
├── astra-child/
│   ├── style.css
│   ├── functions.php
│   └── templates/
│       └── page-generator.php
└── plugins/
    └── smn-generator-core/
        ├── smn-generator-core.php
        ├── includes/
        │   ├── class-smn-activator.php
        │   ├── class-smn-rest-controller.php
        │   ├── class-smn-dataset.php
        │   ├── class-smn-generator.php
        │   └── class-smn-shortcode.php
        ├── data/
        │   ├── names-zulu.json
        │   ├── names-xhosa.json
        │   ├── names-afrikaans.json
        │   ├── names-afrikaans-modern.json
        │   ├── names-english-sa.json
        │   ├── names-sotho.json
        │   ├── names-sepedi.json
        │   ├── names-tswana.json
        │   ├── names-tsonga.json
        │   ├── names-venda.json
        │   ├── names-ndebele.json
        │   ├── names-swati.json
        │   ├── names-business.json
        │   ├── names-pet.json
        │   └── names-product.json
        └── assets/
            ├── js/
            │   ├── smn-core.js
            │   └── smn-generator.js
            └── css/
                └── smn-styles.css
```

### 5.4 Data Flow

```
User selects category + style
        │
        ▼
┌─────────────────────────────┐
│  Page loads (server-render) │
│  - Initial 10 names in HTML │
│  - SEO content blocks       │
│  - AdSense placeholder divs │
│  - Core JS loads (2KB)     │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  User clicks "Generate"     │
│  - Dynamic import() loads   │
│    generator engine (15KB)  │
│  - Fetch category JSON      │
│    (30-80KB, gzipped)       │
│  - Client-side generation   │
│  - Append to results DOM    │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  User clicks "Generate     │
│  More" (subsequent)         │
│  - No network request       │
│  - Pure JS generation       │
│  - < 10ms response          │
│  - Append to results        │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  User interacts with names  │
│  - Click to copy (clipboard)│
│  - Heart to favorite (LS)   │
│  - WhatsApp share (wa.me)  │
│  - Export (download as TXT) │
└─────────────────────────────┘
```

---

## 6. SEO & Content Strategy

### 6.1 URL Structure

```
/generator/                              ← Main hub page
/generator/business/                     ← Category pillar
/generator/business/modern/              ← Specific generator
/generator/business/vintage/
/generator/business/south-african/
/generator/baby/                         ← Category pillar
/generator/baby/afrikaans/
/generator/baby/afrikaans-modern/
/generator/baby/zulu/
/generator/baby/xhosa/
/generator/baby/english-sa/
/generator/pet/                          ← Category pillar
/generator/pet/dog/
/generator/pet/cat/
/generator/pet/afrikaans/
/generator/product/                      ← Category pillar
/generator/product/modern/
/generator/product/afrikaans/
/generator/software/                     ← Category pillar
/generator/software/modern/
/generator/character/                    ← Category pillar
/generator/character/fantasy/
/generator/character/african/
```

### 6.2 Dynamic SEO via Rank Math Hooks

```php
// Dynamic title based on URL parameters
add_filter('rank_math/frontend/title', function($title) {
    $category = get_query_var('smn_category');
    $style = get_query_var('smn_style');
    if ($category && $style) {
        $labels = smn_get_labels($category, $style);
        return sprintf(
            '%s %s Name Generator — Free %s Names - SayMyName',
            $labels['style'],
            $labels['category'],
            $labels['style']
        );
    }
    return $title;
});

// Dynamic meta description
add_filter('rank_math/frontend/description', function($description) {
    $category = get_query_var('smn_category');
    $style = get_query_var('smn_style');
    if ($category && $style) {
        $labels = smn_get_labels($category, $style);
        return sprintf(
            'Generate unique %s %s names with our free name generator. Find the perfect %s name for your %s in seconds. 100%% free, no signup required.',
            $labels['style'],
            $labels['category'],
            $labels['style'],
            $labels['category']
        );
    }
    return $description;
});
```

### 6.3 Schema Markup Strategy

**Per generator page, inject three schema types:**

1. **HowTo Schema** — Describes the generation process (3 steps)
2. **FAQ Schema** — 3-5 FAQs specific to the category+style combination
3. **ItemList Schema** — The first 10 generated names (server-rendered)

All injected via `rank_math/json_ld` filter.

### 6.4 Content Block Strategy

Each category+style combination gets pre-written:
- **H1 heading** — e.g., "Modern Zulu Baby Name Generator"
- **Intro paragraph** — 50-100 words explaining the naming style
- **Tips section** — 3-5 tips for choosing names in this category
- **FAQ section** — 3-5 questions with answers
- **Example names** — 5-10 curated examples
- **Related generators** — Links to 3-5 related category+style combos

Content is stored in a PHP array within the plugin, keyed by `{category}_{style}`.

### 6.5 Keyword Strategy

**Primary keyword clusters:**
- `[style] [category] name generator` — e.g., "modern Zulu baby name generator"
- `[style] [category] names` — e.g., "vintage Afrikaans business names"
- `South African [category] names` — e.g., "South African baby names"
- `[language] [category] names` — e.g., "Xhosa business names"

**Long-tail opportunities (low competition):**
- "Modern Afrikaans baby names for boys"
- "Zulu business name ideas for startups"
- "Vintage British pet names for dogs"
- "Traditional Sotho baby girl names"
- "Unique Sepedi business names"
- "Old school Afrikaans family business names"

### 6.6 Internal Linking Architecture

```
Homepage
  └── /name-generator/ (hub — links to all categories)
       ├── /generator/business/ (category index)
       │    ├── /generator/business/modern/
       │    ├── /generator/business/vintage/
       │    ├── /generator/business/afrikaans/
       │    └── /generator/business/zulu/
       ├── /generator/baby/ (category index)
       │    ├── /generator/baby/afrikaans/
       │    ├── /generator/baby/zulu/
       │    ├── /generator/baby/xhosa/
       │    └── /generator/baby/english-sa/
       ├── /generator/pet/
       ├── /generator/product/
       └── /blog/ (content hub)
            ├── /blog/how-to-choose-business-name-sa/
            ├── /blog/afrikaans-baby-name-trends-2026/
            └── /blog/zulu-business-naming-traditions/
```

---

## 7. AdSense Monetization Strategy

### 7.1 Ad Placement Layout

```
┌─────────────────────────────────────────────┐
│  Header / Navigation (NO ADS)              │
├─────────────────────────────────────────────┤
│  [Ad Unit 1: 728x90 Leaderboard]           │ ← #smn-ad-above-fold
│  (min-height: 90px, 150px from tool)       │
├─────────────────────────────────────────────┤
│  Page Title + Intro Content                 │
├─────────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐    │
│  │ Generator Widget (AD-FREE ZONE)    │    │
│  │ - Category selector                 │    │
│  │ - Style pills                       │    │
│  │ - Generate button                   │    │
│  └─────────────────────────────────────┘    │
├─────────────────────────────────────────────┤
│  [Ad Unit 2: 300x250 Rectangle]            │ ← #smn-ad-in-feed
│  (min-height: 250px)                       │
├─────────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐    │
│  │ Results Grid (AD-FREE ZONE)         │    │
│  │ - Name cards with copy/fav/share    │    │
│  │ - "Generate More" button            │    │
│  └─────────────────────────────────────┘    │
├─────────────────────────────────────────────┤
│  [Ad Unit 3: 728x90 or 300x250]            │ ← #smn-ad-below-results
│  (min-height: 90px)                        │
├─────────────────────────────────────────────┤
│  SEO Content Block (tips, FAQs, related)    │
├─────────────────────────────────────────────┤
│  Footer                                     │
└─────────────────────────────────────────────┘
+ Auto Ads (anchor ad on mobile only)
```

### 7.2 AdSense Policy Compliance

**Critical rules for tool sites:**
1. **150px minimum distance** between any interactive tool element and any ad
2. **No ads inside the tool container** — the generator form and results area are ad-free zones
3. **Clear visual separation** — borders, background colors, or spacing divs between tool and ads
4. **Reserve ad space** with CSS `min-height` to prevent CLS
5. **No custom sticky ads** — use Google's official Auto Ads anchor ads only
6. **No ads mimicking tool UI** — ads must not look like "Copy" or "Share" buttons
7. **Max 3 display ad units** + Auto Ads per page

### 7.3 CLS Prevention

```css
#smn-ad-above-fold {
    min-height: 90px;
    width: 100%;
    max-width: 728px;
    margin: 20px auto;
}

#smn-ad-in-feed {
    min-height: 250px;
    width: 100%;
    max-width: 336px;
    margin: 20px auto;
}

#smn-ad-below-results {
    min-height: 90px;
    width: 100%;
    max-width: 728px;
    margin: 20px auto;
}
```

### 7.4 Auto Ads Configuration

- **Enable:** Anchor ads (sticky bottom) — excellent for mobile
- **Enable:** In-page display ads — let Google fill gaps
- **Enable:** In-article ads — if content blocks are substantial
- **DISABLE:** Vignette/interstitial ads — terrible UX on tool pages
- **DISABLE:** Overlay ads — can cover the generator interface
- **Exclude:** The `.smn-generator-widget` container from Auto Ads placement

---

## 8. File-by-File Implementation Plan

### 8.1 Plugin: `smn-generator-core/smn-generator-core.php`

**Purpose:** Plugin bootstrap, activation hooks, constants, autoloader

**Key elements:**
- Plugin header with metadata
- Define `SMN_VERSION`, `SMN_PLUGIN_DIR`, `SMN_PLUGIN_URL` constants
- Autoloader for `includes/` classes
- Activation hook: flush rewrite rules, set default options
- Deactivation hook: flush rewrite rules
- Hook into `init` to register REST routes
- Hook into `wp_enqueue_scripts` to enqueue assets
- Register `[saymyname_generator]` shortcode

### 8.2 Plugin: `includes/class-smn-activator.php`

**Purpose:** Activation/deactivation logic

**Key elements:**
- `activate()`: Set default options, flush rewrite rules
- `deactivate()`: Clean up, flush rewrite rules
- `register_generator_rewrite_rules()`: Add rewrite rules for `/generator/{category}/{style}/`

### 8.3 Plugin: `includes/class-smn-rest-controller.php`

**Purpose:** REST API endpoint for name generation

**Key elements:**
- `register_routes()`: Register `/saymyname/v1/generate`
- `generate_names()`: Callback that returns JSON of generated names
- Parameters: `category` (required), `style` (required), `count` (default 10)
- Permission callback: `__return_true` (public endpoint)
- Schema validation for all parameters
- Uses `Smn_Generator` class for server-side generation

### 8.4 Plugin: `includes/class-smn-dataset.php`

**Purpose:** Load and cache name datasets

**Key elements:**
- `get_dataset($category, $style)`: Load JSON file, cache via Transient API
- `get_all_categories()`: Return list of available categories
- `get_all_styles()`: Return list of available styles
- `get_dataset_path($category, $style)`: Build path to JSON file
- `get_content_blocks($category, $style)`: Return pre-written SEO content
- `get_labels($category, $style)`: Return human-readable labels

### 8.5 Plugin: `includes/class-smn-generator.php`

**Purpose:** Server-side name generation algorithm

**Key elements:**
- `generate($category, $style, $count, $existing = [])`: Main generation method
- Algorithm: 70% prefix+suffix blend, 30% curated full names
- Duplicate prevention via `$existing` array
- Returns array of `{name, meaning}` objects
- Fallback: If dataset is empty, return curated default names

### 8.6 Plugin: `includes/class-smn-shortcode.php`

**Purpose:** `[saymyname_generator]` shortcode handler

**Key elements:**
- `render($atts)`: Output the generator HTML
- Server-renders first 10 names
- Includes AdSense placeholder divs
- Outputs SEO content blocks
- Progressive enhancement: form-based fallback
- Parameters: `category` (default: 'business'), `style` (default: 'modern')

### 8.7 Plugin: `assets/js/smn-core.js`

**Purpose:** Core UI JavaScript (loaded synchronously, ~2KB)

**Key elements:**
- Category selector change handler
- Style pills click handler
- "Generate" button click handler (triggers dynamic import)
- Loading state management
- URL parameter sync (update URL on category/style change)
- localStorage preference save/load
- Dark mode toggle
- Responsive behavior

### 8.8 Plugin: `assets/js/smn-generator.js`

**Purpose:** Generator engine (loaded via dynamic import, ~15KB)

**Key elements:**
- `generateNames(category, style, count, existing)`: Client-side generation
- Prefix/suffix blending algorithm
- Duplicate prevention
- Results rendering (document fragment, no layout thrashing)
- "Generate More" accumulation
- Click-to-copy (Clipboard API)
- Favorites (localStorage)
- WhatsApp sharing (wa.me links)
- Export as TXT
- Name meaning display
- Gender filter (for baby names)
- Name length filter
- Starting letter filter

### 8.9 Plugin: `assets/css/smn-styles.css`

**Purpose:** Generator styling (theme-agnostic, ~8KB)

**Key elements:**
- CSS custom properties for theming (colors, spacing, fonts)
- `.smn-generator` — main container
- `.smn-generator-widget` — the interactive widget
- `.smn-category-selector` — category pills/dropdown
- `.smn-style-pills` — style modifier pills
- `.smn-generate-btn` — primary CTA button
- `.smn-results-grid` — results display grid
- `.smn-name-card` — individual name card
- `.smn-copy-btn`, `.smn-fav-btn`, `.smn-share-btn` — action buttons
- `.smn-ad-container` — AdSense placeholder styling
- `.smn-seo-content` — SEO content block styling
- `.smn-dark-mode` — dark mode overrides
- Responsive breakpoints (mobile, tablet, desktop)
- Animations (fade in, copy feedback, heart toggle)

### 8.10 Astra Child: `style.css`

**Purpose:** Child theme header

**Key elements:**
- Theme header: `Template: astra`
- Import parent theme styles
- Minimal overrides for generator page

### 8.11 Astra Child: `functions.php`

**Purpose:** Child theme functions

**Key elements:**
- Enqueue parent theme styles
- Register custom page template
- Add rewrite rules for generator URLs
- Rank Math SEO hooks (dynamic title, description, schema)
- Flush rewrite rules on theme activation

### 8.12 Astra Child: `templates/page-generator.php`

**Purpose:** Custom page template for generator

**Key elements:**
- Template header: `Template Name: Name Generator`
- Full-width layout (Astra-compatible)
- Call `[saymyname_generator]` shortcode
- Enqueue plugin assets
- Output SEO content blocks
- Output AdSense placeholder divs

---

## 9. Phased Development Roadmap

### Phase 0: Foundation (Week 1)
- [ ] Set up WordPress + Astra Child theme
- [ ] Create Git repository
- [ ] Install Rank Math SEO
- [ ] Set up local dev environment
- [ ] Create plugin skeleton

### Phase 1: Core Generator (Week 1-2)
- [ ] Create name datasets (start with 5 languages: Zulu, Xhosa, Afrikaans, English SA, Sotho)
- [ ] Implement `Smn_Generator` class (PHP + JS algorithms)
- [ ] Implement REST API endpoint
- [ ] Build generator UI (HTML + CSS)
- [ ] Implement core JS (generate, copy, generate more)
- [ ] Create Astra Child page template
- [ ] Implement shortcode
- [ ] Test: generation works, copy works, no-JS fallback works

### Phase 2: SEO & Content (Week 2-3)
- [ ] Create SEO content blocks for all category+style combos
- [ ] Implement Rank Math dynamic title/description hooks
- [ ] Implement schema markup (HowTo, FAQ, ItemList)
- [ ] Set up clean URL rewrite rules
- [ ] Create sitemap structure
- [ ] Write initial blog posts (3-5 articles)
- [ ] Test: Google Search Console, PageSpeed Insights

### Phase 3: AdSense Integration (Week 3)
- [ ] Add AdSense placeholder divs with CLS-safe CSS
- [ ] Install Site Kit by Google
- [ ] Configure Auto Ads (anchor only, no vignette)
- [ ] Set up manual ad placements
- [ ] Test: CLS score, ad viewability, no accidental clicks
- [ ] Apply for AdSense (if not already approved)

### Phase 4: Enhanced Features (Week 3-4)
- [ ] Name meaning display
- [ ] Favorites (localStorage)
- [ ] WhatsApp sharing
- [ ] Gender filter (baby names)
- [ ] Name length filter
- [ ] Starting letter filter
- [ ] Dark mode toggle
- [ ] Name history
- [ ] Export as TXT
- [ ] Pronunciation guides

### Phase 5: Expansion (Month 2)
- [ ] Add remaining language datasets (Sepedi, Setswana, Tsonga, Venda, Ndebele, Swati)
- [ ] Add business name datasets (industry-specific)
- [ ] Add pet name datasets (breed-specific)
- [ ] Add product/software name datasets
- [ ] Add character name datasets
- [ ] Expand SEO content blocks
- [ ] Write 10+ blog posts
- [ ] Build internal linking structure

### Phase 6: Advanced Features (Month 2-3)
- [ ] Multi-language UI (English, Afrikaans, isiZulu)
- [ ] Export as CSV
- [ ] Similar name suggestions
- [ ] Shareable permalinks
- [ ] Name popularity indicators
- [ ] Batch generation (50 names)
- [ ] .co.za domain checking
- [ ] PWA support (offline generation)

### Phase 7: Growth & Moats (Month 3+)
- [ ] Pronunciation audio recordings
- [ ] Community features (ratings, comments)
- [ ] SA domain registrar partnership
- [ ] AI-powered name suggestions
- [ ] Content marketing at scale
- [ ] Link building for SA-specific queries

---

## 10. Name Dataset Strategy

### 10.1 Dataset Structure

```json
{
  "version": 1,
  "meta": {
    "language": "zulu",
    "total_prefixes": 150,
    "total_suffixes": 100,
    "total_full_names": 300,
    "total_meanings": 300
  },
  "prefixes": [
    "Nkosi", "Mandla", "Sipho", "Thando", "Buhle",
    "Sibusiso", "Bongani", "Dumisani", "Jabulani", "Sifiso",
    "Siyabonga", "Themba", "Vusumuzi", "Xolani", "Nkosana",
    "Nkululeko", "Sandile", "Zwelakhe", "Mthunzi", "Lungelo"
  ],
  "suffixes": [
    "kazi", "phiwe", "sizwe", "khaya", "nathi",
    "thando", "lutho", "muzi", "cele", "hle",
    "nkosi", "buhle", "zwane", "khule", "nani"
  ],
  "full_names": [
    {"name": "Nkosazana", "meaning": "woman of the government"},
    {"name": "Mandlakazi", "meaning": "strength of the nation"},
    {"name": "Siphiwe", "meaning": "we have been given"},
    {"name": "Thandolwethu", "meaning": "our love"},
    {"name": "Buhlebethu", "meaning": "our beauty"}
  ],
  "meanings": {
    "Nkosi": "chief/ruler",
    "Mandla": "strength",
    "Sipho": "gift",
    "Thando": "love",
    "Buhle": "beauty/kindness"
  }
}
```

### 10.2 Combinatorial Math

| Language | Prefixes | Suffixes | Combinations | Full Names | Total Unique |
|----------|----------|----------|-------------|------------|-------------|
| Zulu | 150 | 100 | 15,000 | 300 | 15,300 |
| Xhosa | 120 | 80 | 9,600 | 250 | 9,850 |
| Afrikaans (Old) | 100 | 80 | 8,000 | 200 | 8,200 |
| Afrikaans (Modern) | 80 | 60 | 4,800 | 150 | 4,950 |
| English SA | 100 | 80 | 8,000 | 200 | 8,200 |
| Sotho | 100 | 70 | 7,000 | 200 | 7,200 |
| Sepedi | 80 | 60 | 4,800 | 150 | 4,950 |
| Setswana | 80 | 60 | 4,800 | 150 | 4,950 |
| Tsonga | 60 | 50 | 3,000 | 100 | 3,100 |
| Venda | 60 | 50 | 3,000 | 100 | 3,100 |
| Ndebele | 60 | 50 | 3,000 | 100 | 3,100 |
| Swati | 60 | 50 | 3,000 | 100 | 3,100 |
| **Total** | **1,050** | **790** | **74,000** | **2,000** | **~76,000** |

### 10.3 Data Sourcing Plan

**Phase 1 (MVP — 5 languages):**
- Zulu: Wikipedia + BehindTheName + academic sources
- Xhosa: Wikipedia + BehindTheName + academic sources
- Afrikaans (Old + Modern): Wikipedia + StatsSA + genealogical records
- English SA: BehindTheName + StatsSA
- Sotho: Wikipedia + BehindTheName

**Phase 2 (Expansion — 6 more languages):**
- Sepedi, Setswana, Tsonga, Venda, Ndebele, Swati
- Sources: Academic papers, SA language boards, cultural organizations

**Quality assurance:**
- Each name verified by native speaker or authoritative source
- Meanings verified against multiple sources
- Prefix/suffix combinations tested for cultural authenticity
- No offensive or inappropriate combinations

---

## 11. Performance Targets

| Metric | Target | Measurement |
|--------|--------|-------------|
| First Contentful Paint (FCP) | < 1.5s | Lighthouse |
| Largest Contentful Paint (LCP) | < 2.0s | Lighthouse |
| Time to Interactive (TTI) | < 3.0s | Lighthouse |
| Cumulative Layout Shift (CLS) | < 0.1 | Lighthouse |
| Total Blocking Time (TBT) | < 200ms | Lighthouse |
| First Input Delay (FID) | < 100ms | Chrome UX Report |
| Page load (shared host) | < 2.0s | WebPageTest |
| First name generation | < 100ms | Custom timing |
| Subsequent generations | < 10ms | Custom timing |
| JSON dataset load | < 500ms | Network tab |
| PageSpeed Score | 90+ | PageSpeed Insights |

---

## 12. Risk Assessment & Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| **Thin content penalty** | Medium | High | Pre-written unique content per page, avoid auto-generated gibberish |
| **AdSense policy violation** | Medium | High | 150px distance from tool, no ads in tool area, CLS-safe placeholders |
| **Low traffic initially** | High | Medium | Focus on long-tail SA keywords, content marketing, social sharing |
| **Name data inaccuracies** | Medium | High | Verify with native speakers, cite sources, allow corrections |
| **Duplicate content across pages** | Medium | Medium | Unique content per combo, canonical URLs, noindex thin pages |
| **Performance on shared hosting** | Medium | Medium | Client-side generation, minimal PHP, aggressive caching |
| **Competitor copies concept** | Low | Medium | Build moats: audio pronunciations, community, SA partnerships |
| **Google algorithm update** | Medium | High | Diversify traffic sources, build email list, focus on quality |
| **Domain authority building** | High | Medium | Content marketing, guest posts, SA business directories |
| **Mobile UX issues** | Low | High | Mobile-first design, test on real devices, touch-friendly UI |

---

## Appendix A: Category × Style Matrix

| Category | Styles Available | Est. Pages |
|----------|-----------------|-----------|
| Business | Modern, Vintage, British, Afrikaans, Zulu, Xhosa, Sotho, English SA | 8 |
| Baby | Modern, Traditional, Afrikaans, Afrikaans Modern, Zulu, Xhosa, Sotho, Sepedi, Setswana, English SA, Tsonga, Venda, Ndebele, Swati | 14 |
| Pet | Modern, Vintage, Afrikaans, Zulu, Xhosa, English SA | 6 |
| Product | Modern, Vintage, Afrikaans, English SA | 4 |
| Software | Modern, Tech, Afrikaans, English SA | 4 |
| Character | Fantasy, African, Zulu, Xhosa, Afrikaans, English SA | 6 |
| **Total** | | **42+ pages** |

## Appendix B: Content Block Template

```php
// Per category+style combination
$content = [
    'business_modern' => [
        'h1' => 'Modern Business Name Generator',
        'intro' => 'Looking for a sleek, contemporary name for your business? Our modern business name generator creates names that convey innovation, professionalism, and forward-thinking. Perfect for startups, tech companies, and modern brands in South Africa.',
        'tips' => [
            'Keep it short — 1-2 syllables is ideal for memorability',
            'Avoid hard-to-spell words — make it easy for customers to find you',
            'Consider .co.za domain availability before finalizing',
            'Test pronunciation in English and Afrikaans for SA audiences',
        ],
        'faqs' => [
            ['q' => 'What makes a business name "modern"?', 'a' => 'Modern business names often use shortened words, unique spellings, and tech-inspired prefixes like Nex-, Vel-, or Lux-. They tend to be shorter, more abstract, and globally appealing.'],
            ['q' => 'Can I use these names for my South African business?', 'a' => 'Yes! All names are generated with South African business registration in mind. We recommend checking the CIPC name database and .co.za domain availability before registering.'],
        ],
        'examples' => ['NexaCore', 'VeloPulse', 'LuxVentures', 'ApexWorks', 'NovaForge'],
        'related' => [
            ['url' => '/generator/business/vintage/', 'label' => 'Vintage Business Names'],
            ['url' => '/generator/business/afrikaans/', 'label' => 'Afrikaans Business Names'],
        ],
    ],
];
```

## Appendix C: WordPress Plugin Checklist

- [ ] Plugin header with proper metadata
- [ ] All functions/classes prefixed with `smn_`
- [ ] Namespace: `SayMyName\Generator`
- [ ] No jQuery dependency
- [ ] No database queries on frontend
- [ ] All output escaped (`esc_html`, `esc_attr`, `wp_kses`)
- [ ] All input sanitized (`sanitize_text_field`, `absint`)
- [ ] REST API with schema validation
- [ ] Shortcode for theme-agnostic embedding
- [ ] Transient API for dataset caching
- [ ] Deferred JS loading
- [ ] CSS custom properties for theming
- [ ] Responsive design (mobile-first)
- [ ] Accessibility (ARIA labels, keyboard navigation)
- [ ] Translation-ready (`__()`, `_e()`)
- [ ] Uninstall.php cleanup
- [ ] README.txt with installation instructions

---

> **Document Version:** 1.0
> **Last Updated:** June 2026
> **Next Steps:** Begin Phase 0 — Set up WordPress, Astra Child, and create plugin skeleton
