# 2026-06-28 Leaderboard, Name Input, Animal Sounds & Tooltips

## Changes Made

### Leaderboard System (Fully Working)
- **Created `leaderboard.js`** — new JS file that fetches scores from `/howzitza/v1/leaderboard` API and renders a styled table with rank, player name (with character avatar), Gees, Ubuntu, Changer scores
- **Fixed DB table** — `wp_howzitza_scores` table didn't exist on live server (activation hook never ran). Created it manually with `character_slug` column
- **Added `character_slug` column** to `howzitza_scores` table in schema and `save-score` API
- **Leaderboard page** (`/leaderboard/`) now shows scores or "No scores yet!" empty state

### Name Input (No Popups)
- **Removed `prompt()` calls** from both `game.js` and `quiz.js` — no more popup dialogs
- **Added inline name input** on results screen — appears after seeing your character results
- **"Save to Leaderboard" button** — optional, user types name and clicks to save
- **Feedback** — input turns green with "✅ Saved!" after saving
- Works on both scenario game (`/play/`) and personality test (`/personality-test/`)

### Animal Sounds on Quiz Results
- **Added `triggerSurpriseAnimal()` to `quiz.js`** — 50% chance springbok or hadeda overlay with Web Audio sound
- Springbok: square-wave snort-bark (two bursts)
- Hadeda: sawtooth HA-HA-HA squawk (three bursts)
- Overlay auto-dismisses after 2.5s

### Tooltips for Stats
- **Gees** — `title="Gees (from Afrikaans) = your spirit, vibe, and fun energy"`
- **Ubuntu** — `title="Ubuntu (Nguni philosophy) = your community spirit and humanity"`
- **Changer** — `title="Changer (SA slang) = your money, how much cash you've got"`
- Applied to both scenario game stat bars and leaderboard table headers

### Inline Scenarios (Faster Loading)
- **Scenarios now inlined** via `wp_localize_script` in `howzitza-game-engine.php`
- JS reads `window.howzitzaData.scenarios` directly — no network fetch for the selector screen
- Falls back to API fetch if inline data not available
- 15 scenarios baked into the page HTML

### Files Changed
- `wp-content/mu-plugins/assets/game.js` — name input, tooltips, inline scenarios, saveLeaderboardName
- `wp-content/mu-plugins/assets/quiz.js` — animal sounds, name input, saveLeaderboardName
- `wp-content/mu-plugins/assets/leaderboard.js` — NEW: leaderboard rendering
- `wp-content/mu-plugins/howzitza-game-engine.php` — inline scenarios, character_slug in save-score API, DB schema

## Git
- Branch: `feature/initial-site-build`
- Commit: `a5eb327` — "Leaderboard, name input, animal sounds, tooltips, inline scenarios - full gameplay flow"
