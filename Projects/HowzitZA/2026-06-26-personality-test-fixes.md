# Personality Test Fixes — 2026-06-26

## Problem
The personality test results showed emoji (🥩, 💼, etc.) instead of the actual character illustrations. The quiz also had no answer buttons — questions existed but choices were empty.

## Root Causes

### 1. Empty question choices
All 25 `howzitza_question` posts had `_howzitza_q_choices` meta field set to `[]`. The questions were created with titles and content but the choices (with character scores) were never saved. The `save_post` hook checks for `$_POST['howzitza_q_choices']` which only works via the admin form, not REST API.

### 2. Emoji instead of images
Both `quiz.js` and `game.js` had a `CHAR_EMOJI` mapping that rendered emoji. The 10 character images were uploaded to the media library (IDs 162-171) but:
- Never set as featured images on character posts
- Never referenced in the JavaScript

### 3. Scenario choices as JSON strings
`get_post_meta` returned raw JSON strings for `_howzitza_choices` instead of parsed arrays, breaking the scenario detail API.

## Fixes Applied

### PHP (`howzitza-game-engine.php`)
- Added `howzitza_register_rest_meta()` hooked to `rest_api_init` — registers all `_howzitza_*` meta fields for REST API access
- Added `/howzitza/v1/save-question-choices` POST endpoint — saves `_howzitza_q_choices` directly to DB via `update_post_meta`
- Fixed `howzitza_format_scenario()` — handles JSON strings from `get_post_meta` with `is_string()` check + `json_decode()`

### JavaScript (`quiz.js`, `game.js`)
- Replaced `CHAR_EMOJI` with `CHAR_IMAGES` mapping slugs to media library URLs
- Results display now renders `<img>` tags (120×120, circular crop, colored border) instead of emoji

### Data
- Populated all 25 questions with 4 choices each (100 total) via the new save endpoint
- Each choice has character scores for 2 characters

## Files Changed
- `wp-content/mu-plugins/howzitza-game-engine.php` — +84 lines (meta registration, save endpoint, JSON parsing fix)
- `wp-content/mu-plugins/assets/quiz.js` — emoji→images, results display
- `wp-content/mu-plugins/assets/game.js` — emoji→images, results display

## Git
Commit: `3b6ec58` on `feature/initial-site-build`

---

# Scenario Images Fix — 2026-06-26 (later)

## Problem
The scenario selector cards showed category emoji (🏉, 🥩, 🚗, etc.) instead of the unique per-scenario images that were generated and uploaded to the server. The images existed at `/wp-content/uploads/2026/06/scenario-*.png` but were never wired into the code.

## Root Cause
The `game.js` `showScenarioSelector()` function called `getCategoryEmoji(s.category)` which returned emoji based on the scenario's category (Sport→🏉, Food→🥩, etc.). The 15 scenario images were uploaded via FTP but the JavaScript was never updated to reference them.

## Fix Applied
- Added `SCENARIO_IMAGES` mapping in `game.js` — maps each scenario slug to its image URL
- Updated `showScenarioSelector()` to render `<img>` tags (140px height, cover, rounded top corners) instead of emoji
- Updated `showScenario()` to show the scenario image above the question in the detail view
- Fixed all image URLs to use relative paths (`/wp-content/uploads/...`) instead of hardcoded `5minutes.co.za` domain (images are on howzitza.co.za, not 5minutes.co.za)
- Same URL fix applied to `CHAR_IMAGES` in both `game.js` and `quiz.js`

## Files Changed
- `wp-content/mu-plugins/assets/game.js` — +43 lines (SCENARIO_IMAGES, card rendering, detail view)
- `wp-content/mu-plugins/assets/quiz.js` — URL fix for character images

## Git
Commit: `1f248e5` on `feature/initial-site-build`

---

# Environmental Modifiers & Surprise Reveal Restore — 2026-06-26

## Problem
The Eskom Se Push countdown, Load Shedding overlay, Hadeda shuffle, Traffic Jam notice, and the surprise animal reveal (Springbok/Hadeda with sound) were all missing from the scenarios. The code existed in `game.js` but the `env_mods` field was returning a JSON string instead of a parsed array, so the `if (scenario.env_mods && scenario.env_mods.length > 0)` check was never true.

## Root Cause
Same JSON string issue as the scenario choices — `get_post_meta` returned `["eskoms_push","hadeda_scream"]` as a string, not an array. The `howzitza_format_scenario()` function only fixed `_howzitza_choices` but not `_howzitza_env_mods`.

## Fixes Applied
- `howzitza_format_scenario()`: added `is_string()` + `json_decode()` for `_howzitza_env_mods` (same pattern as choices fix)
- `game.js`: added `triggerSurpriseAnimal()` — 50% chance after results showing Springbok or Hadeda with pop animation and synthesized sound
- `game.js`: added `pointer-events:none` to Eskom Push and Load Shedding overlays so clicks pass through
- `game.css`: added `howzitza-shake` (violent multi-axis shake) and `howzitza-pop` (scale-in) animations
- Version bumped to `1.0.2`

## Files Changed
- `wp-content/mu-plugins/howzitza-game-engine.php` — env_mods JSON parsing fix
- `wp-content/mu-plugins/assets/game.js` — surprise animal reveal, pointer-events on overlays
- `wp-content/mu-plugins/assets/game.css` — shake + pop animations

## Git
Commit: `2cd611f` on `feature/initial-site-build`

---

# 25 Question Images — 2026-06-26

## Problem
The personality test had no images for each question. The 25 questions showed only text and choice buttons.

## Fix Applied
- Generated 25 unique flat vector illustrations via FAL.ai Flux.2 Klein
- Each image matches a specific question theme (braai, load shedding, WhatsApp, etc.)
- Prompts use "flat vector illustration, solid colors no gradients" for consistent style
- Each prompt includes specific South African cultural references
- Uploaded to `/wp-content/uploads/2026/06/question-q01-braai.png` through `question-q25-gatsby.png`
- Added `QUESTION_IMAGES` mapping in `quiz.js` (by question ID)
- Updated `showQuestion()` to display image above question text
- Added `.howzitza-quiz-image` CSS class (full width, 200px max height, rounded corners)

## Files Changed
- `wp-content/mu-plugins/assets/quiz.js` — +28 lines (QUESTION_IMAGES mapping, image in showQuestion)
- `wp-content/mu-plugins/assets/game.css` — +8 lines (.howzitza-quiz-image class)

## Git
Commit: `474ce1a` on `feature/initial-site-build`
