# SayMyName.co.za — Technical Architecture

## Architecture Overview

```
Browser (Client)
├── smn-core.js (2KB, sync) — UI handlers, state mgmt, ad placeholders
├── smn-generator.js (15KB, dynamic import) — Generation algorithms
└── Category JSON (30-80KB each, lazy-loaded, gzip-compressed)
        │
        ▼
WordPress REST API — /wp-json/saymyname/v1/generate
        │
        ▼
smn-generator-core Plugin
├── REST Controller
├── Dataset Loader (Transient API cached)
└── Server Generator (PHP fallback)
        │
        ▼
Caching Layer
├── Transient API (dataset cache)
├── Page Cache (WP Super Cache / W3 Total Cache)
└── Object Cache (Redis if available)
        │
        ▼
Astra Child Theme — page-generator.php
├── Full-width layout
├── Enqueues plugin assets
└── Dynamic SEO content blocks
```

## Key Architectural Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Generation approach | Hybrid: Server-first batch, then client-side | SEO requires names in HTML; UX requires instant subsequent generations |
| API method | WordPress REST API | Cacheable, modern, works with caching plugins |
| Data format | Flat JSON arrays per category | Fastest JS access, gzip-compresses well |
| Client storage | sessionStorage + localStorage | sessionStorage for session state, localStorage for preferences |
| Code splitting | Dynamic import() for generator engine | Core JS is 2KB, generator is 15KB lazy-loaded |
| CSS approach | Custom .smn-* classes, no framework | Theme-agnostic, minimal, no dependencies |
| Plugin structure | No DB queries, no jQuery dependency | Maximum performance, zero database load |
| URL structure | Clean URLs via WordPress rewrite rules | SEO-friendly, indexable per combination |
| Progressive enhancement | Form-based fallback with noscript | Works without JS at basic level |

## Data Flow

1. Page loads → Server renders first 10 names in HTML + SEO content + AdSense placeholders
2. Core JS loads (2KB) → Sets up UI handlers
3. User clicks "Generate" → Dynamic import() loads generator engine (15KB)
4. Fetch category JSON (30-80KB, gzipped) → Client-side generation
5. Append results to DOM → No page reload
6. Subsequent "Generate More" → Pure JS, no network, <10ms response
7. User interactions → Copy (clipboard), Favorite (localStorage), Share (wa.me), Export (download)

## Repository Structure
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

## Performance Targets
| Metric | Target |
|--------|--------|
| FCP | < 1.5s |
| LCP | < 2.0s |
| TTI | < 3.0s |
| CLS | < 0.1 |
| TBT | < 200ms |
| Page load | < 2.0s |
| First generation | < 100ms |
| Subsequent generations | < 10ms |
| PageSpeed Score | 90+ |

## Combinatorial Math (Name Generation)
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
