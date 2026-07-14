# HowzitZA Blog Post Image Style Guide

## Art Style
**Cell-shaded vector cartoon** — NOT flat vector. Key characteristics:
- Bold black outlines around all characters and objects
- Cell-shading (distinct blocks of shadow, not smooth gradients)
- Vibrant, saturated colors
- Clean white background
- Exaggerated facial expressions (big smiles, laughter)
- Simplified anatomy with clean rounded shapes

## Image Source
**ALWAYS fal.ai (Flux.2 Klein)** — never Openverse or stock photos.

## Prompt Template
```
"Modern digital cartoon illustration, bold black outlines, cell-shaded, vibrant colors, a diverse group of South African friends of all races - black people, white people, and coloured people - [specific cultural scene], [specific activity], warm friendly atmosphere, South African cultural scene, clean white background, no text no writing no labels no logos no brand names no words anywhere in the image, all clothing is plain solid colors with no writing"
```

## Diversity Requirement
Every image MUST include black, white, AND coloured people. South Africa is ~81% Black, ~8% White, ~9% Coloured, ~2% Indian/Asian. Do NOT default to all-black casts.

## Layout Per Post
- **2 images per post** (matching existing blog pattern)
- **Image 1:** At the very top of the post content, before any text
- **Image 2:** Mid-content, after a major section (e.g. after Food & Drink section)
- Use inline HTML `<figure>` tags, NOT wp:block comment syntax:
  ```html
  <figure class="wp-block-image size-large" style="margin-bottom:20px;">
    <img decoding="async" src="URL" alt="DESCRIPTION" style="width:100%;height:auto;border-radius:12px;" />
  </figure>
  ```

## No Text Rule
Add to every prompt: "no text no writing no labels no logos no brand names no words anywhere in the image, all clothing is plain solid colors with no writing"

## Existing Blog Images (reference)
- `blog-stereotypes-funny.png` — Braai scene with 3 diverse men
- `blog-braai-oom-group.png` — 4 people around a braai table, one playing guitar
- `blog-eskomsespush.png` — Man with phone and candles during load shedding
- `blog-loadshedding-funny.png` — Family of 4 with candles and board games
