# SayMyName.co.za — SEO & Content Strategy

## URL Structure
```
/generator/                              ← Main hub
/generator/business/                     ← Category pillar
/generator/business/modern/              ← Specific generator
/generator/baby/                         ← Category pillar
/generator/baby/afrikaans/
/generator/baby/zulu/
/generator/pet/
/generator/product/
/generator/software/
/generator/character/
```

## Dynamic SEO via Rank Math Hooks
- `rank_math/frontend/title` — Dynamic title per category+style
- `rank_math/frontend/description` — Dynamic meta description
- `rank_math/json_ld` — Dynamic schema injection

## Schema Markup (3 types per page)
1. **HowTo Schema** — 3-step generation process
2. **FAQ Schema** — 3-5 questions per category+style
3. **ItemList Schema** — First 10 generated names

## Content Block Strategy
Each category+style combo gets pre-written:
- H1 heading (e.g., "Modern Zulu Baby Name Generator")
- Intro paragraph (50-100 words)
- Tips section (3-5 tips)
- FAQ section (3-5 Q&A)
- Example names (5-10 curated)
- Related generators (3-5 links)

## Keyword Strategy
**Primary clusters:**
- `[style] [category] name generator` — "modern Zulu baby name generator"
- `[style] [category] names` — "vintage Afrikaans business names"
- `South African [category] names` — "South African baby names"
- `[language] [category] names` — "Xhosa business names"

**Long-tail opportunities (low competition):**
- "Modern Afrikaans baby names for boys"
- "Zulu business name ideas for startups"
- "Vintage British pet names for dogs"
- "Traditional Sotho baby girl names"
- "Unique Sepedi business names"

## Anti-Thin-Content Strategies
1. Unique intro text per page (pre-written)
2. Curated example names (not just algorithm output)
3. Tips & guidance sections
4. FAQ section per combo
5. Related generators (internal links)
6. Blog integration
7. Server-render content (not JS-only)

## Internal Linking Architecture
```
Homepage → /name-generator/ (hub)
  → /generator/business/ (category index)
    → /generator/business/modern/ (specific)
    → /generator/business/vintage/
  → /generator/baby/
    → /generator/baby/afrikaans/
  → /blog/ (content hub)
    → /blog/how-to-choose-business-name-sa/
```

## Category × Style Matrix (42+ pages)
| Category | Styles | Pages |
|----------|--------|-------|
| Business | Modern, Vintage, British, Afrikaans, Zulu, Xhosa, Sotho, English SA | 8 |
| Baby | Modern, Traditional, Afrikaans, Afrikaans Modern, Zulu, Xhosa, Sotho, Sepedi, Setswana, English SA, Tsonga, Venda, Ndebele, Swati | 14 |
| Pet | Modern, Vintage, Afrikaans, Zulu, Xhosa, English SA | 6 |
| Product | Modern, Vintage, Afrikaans, English SA | 4 |
| Software | Modern, Tech, Afrikaans, English SA | 4 |
| Character | Fantasy, African, Zulu, Xhosa, Afrikaans, English SA | 6 |
