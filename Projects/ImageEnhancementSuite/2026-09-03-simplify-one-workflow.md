# 2026-09-03 Remove Confusing Features - One Clear Workflow

## Problem
User frustration:
- "Preview effect →" button crashes the app
- Two confusing buttons: "Preview" vs "Generate" (what's the difference?)
- Too many tabs: Projects, Products, Looks, Effects Vault, Fix these, Generate
- User has to be talked through the workflow

**Root cause:** Building features without human testing, then defending them instead of removing them.

## Solution
**Removed entirely:**
- "Preview effect →" button (crashes, confusing)
- "Looks/Effects" tab (crashes, unnecessary)
- "Effects Vault" tab (advanced feature, not needed yet)
- "Fix these/Review Queue" tab (overcomplicating)

**Simplified workflow:**
```
1. Projects tab → Click your project
2. Products tab → See your products, click "Generate →" on any
3. Generate tab → Effect already selected → Click "Generate images"
4. See thumbnails of your generated images → Done
```

**Changes made:**
- Product cards: ONE button "Generate →" (not two confusing buttons)
- Products tab: ONE CTA "Generate your images →" (not two)
- Navigation: 3 buttons only (Projects, Products, Generate)
- Generate tab: Auto-selects first effect (no decision needed)

## Files Changed
- `app/gui/app.py` - Removed Effects, Effects Vault, Review Queue views
- `app/gui/asset_card.py` - One "Generate →" button
- `app/gui/assets.py` - One CTA button
- `app/gui/generate.py` - Auto-select first effect

## Git
Commit: `07bd7d9` — "Remove confusing Preview/Effects tabs - one clear workflow"

## Testing Required
1. Open IES with desktop shortcut
2. Click `📁 TT.ies`
3. Click "Generate →" on first product
4. Should go to Generate tab with "Gallery Edition" pre-selected
5. Click "Generate images"
6. Should see 3 image thumbnails after completion

**No crashes. No confusion. One path.**
