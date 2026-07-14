# SayMyName.co.za — AdSense Monetization Strategy

## Ad Placement Layout

```
[Header/Nav] — NO ADS
[Ad Unit 1: 728x90 Leaderboard] — #smn-ad-above-fold (min-height: 90px, 150px from tool)
[Page Title + Intro Content]
[Generator Widget] — AD-FREE ZONE
[Ad Unit 2: 300x250 Rectangle] — #smn-ad-in-feed (min-height: 250px)
[Results Grid] — AD-FREE ZONE
[Ad Unit 3: 728x90 or 300x250] — #smn-ad-below-results (min-height: 90px)
[SEO Content Block]
[Footer]
+ Auto Ads (anchor ad on mobile only)
```

## Critical Policy Rules for Tool Sites

1. **150px minimum distance** between any interactive tool element and any ad
2. **No ads inside the tool container** — generator form and results area are ad-free zones
3. **Clear visual separation** — borders, background colors, or spacing divs
4. **Reserve ad space** with CSS `min-height` to prevent CLS
5. **No custom sticky ads** — use Google's official Auto Ads anchor ads only
6. **No ads mimicking tool UI** — ads must not look like "Copy" or "Share" buttons
7. **Max 3 display ad units** + Auto Ads per page

## CLS Prevention

```css
#smn-ad-above-fold { min-height: 90px; width: 100%; max-width: 728px; margin: 20px auto; }
#smn-ad-in-feed { min-height: 250px; width: 100%; max-width: 336px; margin: 20px auto; }
#smn-ad-below-results { min-height: 90px; width: 100%; max-width: 728px; margin: 20px auto; }
```

## Auto Ads Configuration
- **ENABLE:** Anchor ads (sticky bottom) — excellent for mobile
- **ENABLE:** In-page display ads
- **ENABLE:** In-article ads (if content is substantial)
- **DISABLE:** Vignette/interstitial ads — terrible UX on tool pages
- **DISABLE:** Overlay ads — can cover the generator interface
- **EXCLUDE:** `.smn-generator-widget` container from Auto Ads placement

## Best Ad Sizes for Tool Sites
| Size | Position | Notes |
|------|----------|-------|
| 728x90 Leaderboard | Above tool, below results | Best for above-fold |
| 300x250 Rectangle | Between tool/results | Highest CTR position |
| 336x280 Large Rectangle | Between tool/results | Slightly higher CTR |
| 300x600 Half Page | Desktop sidebar | High CPM |
| 320x100 Mobile Banner | Mobile between sections | Via responsive units |

## Revenue Projections
| Stage | Monthly Visits | Est. RPM | Monthly Revenue |
|-------|---------------|----------|-----------------|
| Launch (M1-3) | 1K-5K | $3-5 | $3-25 |
| Growth (M4-6) | 5K-20K | $5-8 | $25-160 |
| Steady (M7-12) | 20K-100K | $8-15 | $160-1,500 |
| Mature (Y2+) | 100K-500K | $10-20 | $1,000-10,000 |
