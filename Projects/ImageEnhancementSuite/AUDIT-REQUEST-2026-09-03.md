# IES Human Audit Request

## Critical Issues Found

### 1. App Crashes on Preview
**User action:** Click "Preview effect →" on a product  
**Expected:** See 11 effect thumbnails loading  
**Actual:** App closes/crashes immediately

**Root cause:** Threading + GUI interaction issue in `effects_view.py`

### 2. Confusing Workflow
User doesn't understand:
- Why there are TWO ways to generate images (Preview vs Generate)
- What the difference is
- Which one they should use

**Current flow:**
1. Open project → Products tab
2. Click "Preview effect →" → Goes to Looks tab → **CRASHES**
3. OR click "Generate" → Goes to Generate tab → Choose effect → Generate

**Should be:**
1. Open project → See products
2. Click "Generate" → Choose effect → See results immediately

### 3. Missing Quality Control
- No human reviewed the code before committing
- Multiple commits with breaking changes
- User has to find and report each bug

## Required Fixes

### Immediate (stop the bleeding)
1. **Remove or fix the crashing preview code** - if it crashes, don't ship it
2. **Test the actual desktop app** - not just headless Python imports
3. **One clear workflow** - not two confusing options

### Process (prevent recurrence)
1. **Human audit BEFORE commit** - another person runs the app and verifies it works
2. **Test checklist:**
   - Open app with desktop shortcut
   - Open existing project
   - Click each button
   - Verify no crashes
   - Verify expected result

## Files to Review
- `app/gui/effects_view.py` - Preview threading code (crashes)
- `app/gui/generate.py` - Image preview code (should work)
- `app/gui/projects.py` - Project opening flow
- `app/gui/assets.py` - Product card buttons

## Auditor Instructions
1. Open IES using desktop shortcut (not command line)
2. Open the "TT" project
3. For EACH product, click:
   - "Preview effect →" - does app crash?
   - "Generate" - does it take you to Generate tab?
4. In Generate tab:
   - Choose "Gallery Edition" effect
   - Click "Generate images"
   - Wait for completion
   - **Do you see thumbnail previews of your 3 images?**
5. Report: What worked, what crashed, what was confusing

---
*Created: 2026-09-03 after user reported app crashes on preview*
