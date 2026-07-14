# Vinci — Image Generation for 5minutes.co.za

**Date:** June 23, 2026
**Tags:** #5minutes #vinci #image-generation #flux #fal #game-graphics

## What is Vinci?

Vinci is an image generation workflow using **Flux.2 Klein (9B)** via FAL.ai, used to create game graphics for the three "Find the..." games on 5minutes.co.za.

## Model Details

- **Model:** `fal-ai/flux-2/klein/9b` (Flux.2 Klein 9B)
- **Provider:** FAL.ai
- **API Key:** `FAL_KEY` in `~/.hermes/.env`
- **Speed:** ~0.5 seconds per image
- **Cost:** ~$0.006 per image
- **Min inference steps:** 4
- **Output format:** PNG

## Generated Images

### Character Art (square_hd, 1024x1024)

| Image | Media ID | Description |
|-------|----------|-------------|
| Springbok | 114 | Cute anime/chibi springbok, brown/white fur, curved horns, big eyes |
| Hadeda Ibis | 115 | Cute anime hadeda, grey-brown feathers, curved ibis beak, iridescent wings |
| Bunny Chow | 116 | Cute anime bunny chow, bread loaf with curry, bunny ears, steam |

### Reveal/Celebration Images (landscape_16_9, 1024x576)

| Image | Media ID | Description |
|-------|----------|-------------|
| Springbok Found | 117 | Leaping in golden savanna, confetti, sparkles |
| Hadeda Found | 118 | Flapping wings in garden, confetti, sparkles |
| Bunny Chow Found | 119 | Balloons, confetti, sparkles |

## Game Pages

| Game | Page ID | Type |
|------|---------|------|
| Find the Springbok | Homepage (41) | `functions.php` injection |
| Find the Hadida | 94 | Elementor HTML widget |
| Find the Bunny Chow | 96 | Elementor HTML widget |

## Sound Effects (Web Audio API)

| Game | Sound | Technical |
|------|-------|-----------|
| Springbok | "Boing!" | Sine 200→800→1200 Hz, triangle pop |
| Hadeda | Loud squawk | Square wave 1500→600→1400→500 Hz wobble |
| Bunny Chow | Cartoon honk | Sine 400→200→500→250 Hz dip |

## Key Lessons

1. **pointer-events:none** on reveal containers blocks the "Play Again" button — always remove it
2. Elementor caches rendered output — always `DELETE /elementor/v1/cache` after changes
3. The Springbok game is slightly wider (1265px vs 1180px) because it's outside the main content wrapper
4. All games use `min-height: 70vh` and `width: 100%` — responsive on mobile
5. Flux.2 Klein produces consistent cute anime/chibi style across different prompts

## Files

- Game code: `/wp-content/themes/astra-child/functions.php`
- Images: `/wp-content/uploads/2026/06/`
- Docs: `/home/m/5minutes.co.za/docs/vinci-image-generation.md`
- Hermes skill: `vinci-image-agent`
