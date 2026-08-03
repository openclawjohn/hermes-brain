# DESIGN_SYSTEM.md — Portfolio of 7 South African Domains

**Last Updated:** 2026-08-03
**Purpose:** Define the visual design system, image standards, content patterns, and per-domain style guides for all 7 portfolio sites.

---

## 🎨 Per-Domain Design Profiles

### 1. whippetqr.com — Free QR Code Generator
| Attribute | Standard |
|-----------|----------|
| **Vibe** | Clean, professional, tech/business |
| **Primary Color** | `#1863DC` (blue) |
| **Secondary** | `#0056A7` (dark blue) |
| **Font** | System font stack (clean sans-serif) |
| **Image Style** | Tech/business stock photos — screenshots, devices, QR codes in real-world settings |
| **Images per Article** | 2 (featured top + in-content ~55% through) |
| **Image Source** | Wikimedia Commons CC-BY, tech/business categories |
| **Image Treatment** | `border-radius:12px; max-width:100%; display:block; margin:0 auto` |
| **Logo** | Custom QR-themed logo |
| **Theme** | Astra + Elementor |

### 2. howzitza.co.za — South African Culture & Games
| Attribute | Standard |
|-----------|----------|
| **Vibe** | Vibrant, fun, cultural, warm |
| **Primary Color** | `#1565C0` (blue) |
| **Secondary** | `#1B5E20` (green), `#25D366` (WhatsApp green) |
| **Font** | System font stack |
| **Image Style** | SA culture photos — people, braais, townships, landscapes, food, music scenes |
| **Images per Article** | 2 (featured top + in-content ~55% through) |
| **Image Source** | Wikimedia Commons CC-BY, SA culture/travel categories |
| **Image Treatment** | `border-radius:12px; max-width:100%; display:block; margin:0 auto` |
| **Logo** | Custom "HowzitZA" branding |
| **Theme** | Astra + Elementor |

### 3. sumza.co.za — SA Calculators & Tools
| Attribute | Standard |
|-----------|----------|
| **Vibe** | Professional, trustworthy, financial |
| **Primary Color** | `#0057B8` (blue), `#0099CC` (teal) |
| **Secondary** | `#00C896` (green accent) |
| **Font** | System font stack |
| **Image Style** | Finance/business/professional photos — calculators, money, documents, office settings |
| **Images per Article** | 2 (featured top + in-content ~55% through) |
| **Image Source** | Wikimedia Commons CC-BY, finance/business categories |
| **Image Treatment** | `border-radius:12px; max-width:100%; display:block; margin:0 auto` |
| **Logo** | Custom SumZA logo (1376x768) |
| **Theme** | Astra + Elementor |

### 4. zadocs.co.za — Free SA Legal Templates
| Attribute | Standard |
|-----------|----------|
| **Vibe** | Clean, professional, legal/formal |
| **Primary Color** | `#7c3aed` (purple) |
| **Secondary** | `#10b981` (green) |
| **Font** | System font stack |
| **Image Style** | Document/legal/professional photos — contracts, documents, gavels, office |
| **Images per Article** | 2 (featured top + in-content ~55% through) |
| **Image Source** | Wikimedia Commons CC-BY, legal/document categories |
| **Image Treatment** | `border-radius:12px; max-width:100%; display:block; margin:0 auto` |
| **Logo** | Custom ZADocs branding |
| **Theme** | Astra + Elementor |

### 5. saymyname.co.za — SA Name Meanings
| Attribute | Standard |
|-----------|----------|
| **Vibe** | Warm, cultural, diverse, family-oriented |
| **Primary Color** | `#046BD2` (blue) |
| **Secondary** | `#3a3a3a` (dark gray) |
| **Font** | System font stack |
| **Image Style** | People/diversity/cultural photos — babies, families, name signs, diverse SA faces |
| **Images per Article** | 2 (featured top + in-content ~55% through) |
| **Image Source** | Wikimedia Commons CC-BY, people/culture categories |
| **Image Treatment** | `border-radius:12px; max-width:100%; display:block; margin:0 auto` |
| **Logo** | Custom SayMyName branding |
| **Theme** | Astra + Elementor |

### 6. 5minutes.co.za — Quick Silly Games
| Attribute | Standard |
|-----------|----------|
| **Vibe** | Fun, playful, light-hearted, energetic |
| **Primary Color** | `#1E4620` (dark green) |
| **Secondary** | `#046BD2` (blue) |
| **Font** | System font stack |
| **Image Style** | Fun/game/SA culture photos — game boards, people playing, SA animals, quirky scenes |
| **Images per Article** | 2 (featured top + in-content ~55% through) |
| **Image Source** | Wikimedia Commons CC-BY, games/fun/SA categories |
| **Image Treatment** | `border-radius:12px; max-width:100%; display:block; margin:0 auto` |
| **Logo** | Custom 5minutes logo (group photo style) |
| **Theme** | Astra + Elementor |

### 7. beanel.com — What Is My IP Address
| Attribute | Standard |
|-----------|----------|
| **Vibe** | Clean, tech, minimalist, trustworthy |
| **Primary Color** | `#0ea5e9` (sky blue), `#004d40` (teal) |
| **Secondary** | `#1863DC` (blue) |
| **Font** | System font stack |
| **Image Style** | Tech/network/security photos — servers, networks, maps, privacy concepts |
| **Images per Article** | 2 (featured top + in-content ~55% through) |
| **Image Source** | Wikimedia Commons CC-BY, tech/network categories |
| **Image Treatment** | `border-radius:12px; max-width:100%; display:block; margin:0 auto` |
| **Logo** | Custom BeaNel branding |
| **Theme** | Astra + Elementor |

---

## 🖼️ Universal Image Standards (All 7 Sites)

### Image Requirements
- **Count:** Exactly 2 per article — no exceptions
- **Position 1:** Featured/hero image at the very top of the article body, full width
- **Position 2:** In-content image at approximately 55-65% through the article body
- **Source:** REAL photos from Wikimedia Commons (CC-BY licensed). NEVER AI-generated images
- **Variety:** The 2 images in one article must show DIFFERENT subjects. Images across different articles on the same site must also look different (vary search keywords per article)
- **Quality:** High resolution, clear, well-lit. No blurry, dark, or low-quality images
- **Width:** Full width of the article content area. Set `width:100%;height:auto;` or use WordPress "full width" image block alignment

### 🚨 Mandatory Visual Inspection Protocol
**Every image MUST be visually inspected with `vision_analyze()` before deployment.**
1. Search for candidate images via Openverse (CC-BY license filter)
2. Visually inspect EVERY candidate — ask: "Inspect EVERY pixel. Is there any text, logo, brand name, URL, watermark, overlay, or advertisement anywhere?"
3. Reject if ANY of these are present: text, logos, watermarks, ads, promotional content
4. Only upload after passing inspection
5. Before reporting completion, visually verify the image renders on the page as a logged-out user

### Image CSS Treatment (All Sites)
```css
img {
    max-width: 100%;
    width: auto;
    height: auto;
    border-radius: 12px;
    object-fit: contain;
    display: block;
    margin: 0 auto;
}
```

---

## 📝 Universal Content Standards (All 7 Sites)

### Article Requirements
- **Minimum word count:** 1,500 words body text (strip HTML before counting)
- **Structure:** Clear H2 headings, logical flow, SA-specific context
- **No duplicate H2 headings** within the same article
- **No boilerplate sections** — no "in the South African Context" or "Why X Matters" repeated text
- **Assign to a real category** — never leave in Uncategorized (Rank Math excludes Uncategorized from sitemap)
- **Navigation label:** Always "Articles" not "Blog"

### SEO Standards
- **Meta titles:** Unique per article, include target keyword, under 60 chars
- **Meta descriptions:** Unique per article, 150-160 chars, include call to action
- **Image alt text:** Every image must have descriptive alt text matching the article topic
- **URL slugs:** Clean, keyword-rich, no stop words. Never use -2 or -3 suffixes
- **Internal links:** Link to other portfolio sites where contextually relevant
- **Canonical URLs:** Set correctly, no duplicate content issues

---

## 🎯 Cross-Site Components

### Portfolio Cross-Links Footer
All 7 sites share a common footer that links to all other portfolio sites:
```css
.portfolio-crosslinks {
    background: #f8f9fa;
    border-top: 1px solid #e9ecef;
    padding: 20px 15px;
    margin-top: 30px;
    text-align: center;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    font-size: 14px;
    line-height: 1.6;
}
```

### Common Plugins (All Sites)
- Rank Math SEO (with Instant Indexing)
- LiteSpeed Cache
- Elementor
- Google Site Kit (Analytics)
- AdSense (meta tag + ads.txt)

---

## 📱 Responsive Breakpoints (All Sites)
| Breakpoint | Width | Behavior |
|------------|-------|----------|
| Desktop | > 1024px | Full layout |
| Tablet | 768px - 1024px | Adjusted grid |
| Mobile | < 768px | Single column, stacked |

---

## 🧪 Quality Checklist (Every Article)
- [ ] 1,500+ words body text
- [ ] 2 real CC-licensed photos (visually inspected)
- [ ] Different subjects in the 2 images
- [ ] No text/logos/watermarks in images
- [ ] Featured image at top, in-content at ~55%
- [ ] Unique meta title and description
- [ ] Image alt text set
- [ ] Clean URL slug (no -2/-3)
- [ ] Assigned to a real category
- [ ] Internal links to other portfolio sites where relevant
- [ ] HTTP 200 when loaded as logged-out user
- [ ] Sitemap includes the new article
- [ ] IndexNow pinged after publishing
