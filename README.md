# Chess Assist

Stockfish chess engine hack for chess.com. Desktop and mobile versions included.

![Chess Assist UI](.github/screenshot.png)

## Features

- **Auto-play** — engine plays the best move automatically each turn with anti-repetition to avoid draw loops
- **Move delay** — random delay before auto-play moves (0–15s) to simulate human thinking time
- **Best move arrows** — green arrow showing the engine's top pick
- **Hidden arrows** — subtle square shading instead of visible arrows for a less obvious look
- **Opponent's best move** — red arrow predicting the opponent's best reply
- **Win % eval bar** — live vertical bar showing win probability from Stockfish evaluation
- **Cheat detection** — real-time CPL, accuracy %, best move rate, and suspicion gauge
- **Opponent / You toggle** — track the opponent's accuracy or your own with full move history
- **Re-analyze game** — replay all moves through the engine to recalculate stats
- **Custom piece skins** — upload custom images for any piece (both colors)
- **Board themes** — custom board background image
- **Tile overlays** — separate images for light and dark squares
- **Adjustable depth** — engine search depth from 1 to 24 (default 16)
- **Customizable keybinds** — assign hotkeys to any toggle
- **Tabbed UI** — Gameplay / Detection / Skins in a draggable panel
- All skins persist in localStorage across sessions
- Runs entirely client-side, no data sent anywhere

## Install — pick one

There are three ways to install Chess Assist. Pick whichever works for you:

### 1. Tampermonkey

Install from Greasy Fork or directly from GitHub:

- **Desktop:** [chess-assist-tampermonkey.js](https://raw.githubusercontent.com/ThatKidGlitchy/chess-hacks/main/chess-assist-tampermonkey.js)
- **Mobile:** [chess-assist-tampermonkey-mobile.js](https://raw.githubusercontent.com/ThatKidGlitchy/chess-hacks/main/chess-assist-tampermonkey-mobile.js)

Open the raw link with Tampermonkey installed and it will offer to install the script. Updates automatically.

### 2. Chrome Extension

Download [`chess-assist-extension.zip`](https://github.com/ThatKidGlitchy/chess-hacks/releases/latest/download/chess-assist-extension.zip) from [releases](https://github.com/ThatKidGlitchy/chess-hacks/releases), unzip, and load as an unpacked extension at `chrome://extensions` with Developer mode on.

### 3. Bookmarklet

See below under each version for the bookmarklet loader URL.

## Desktop (chess-assist.js)

### Usage — Loader Bookmarklet

Always runs the latest version from GitHub:

1. Create a bookmark in your browser with this URL:
   ```
   javascript:void(fetch('https://raw.githubusercontent.com/ThatKidGlitchy/chess-hacks/main/chess-assist.js').then(r=>r.text()).then(eval))
   ```
2. Open a game on chess.com, click the bookmarklet
3. Click the knight icon to open the panel

### Alternative — paste in console

1. Open a game on chess.com
2. Open browser DevTools (F12 → Console)
3. Paste the contents of `chess-assist.js` and press Enter
4. Click the knight icon to open the panel

### Gameplay Tab

- **Depth slider** — engine search depth (1–24, default 16). Higher = stronger but slower
- **Show best move** — green arrow on the board showing the engine's top pick
- **Hidden arrows** — subtle square shading instead of visible arrow (opacity 0.07). Overrides green arrow when both are on
- **Auto-play** — plays the best move automatically each turn with anti-repetition to avoid draw loops
- **Move delay** — toggle on to add a random delay before auto-play moves (min/max sliders, 0–15s). Makes it look like a human thinking
- **Opponent's best** — red arrow (#e04040) showing the opponent's best reply to the current position. Shows on both your turn and the opponent's turn
- **Analyze once** — one-shot analysis without enabling continuous mode. Respects hidden arrows mode
- **Win% eval bar** — vertical bar next to the panel showing live win probability. White fill = white's advantage, updates every position change. Flips when playing as black. Uses a logistic model (centipawn → win%) from Stockfish evaluation
- **Hide all** — hides the entire UI, arrows, shading, and pauses auto-play. Press keybind again to restore everything
- **Keybinds** — click the key badge next to any toggle to assign a hotkey. Supports single keys, modifier combos (Ctrl+G), and modifier-only binds (just Ctrl). Live preview while recording, click away or Escape to cancel. No duplicates allowed
- **Info tooltips** — click the (i) icon next to any toggle to see what it does
- **Pawn promotion** — automatically promotes to queen when a pawn reaches the last rank

### Detection Tab

- **Engine detection** — tracks average centipawn loss (ACPL) in real-time. Compares engine evaluation before and after each move
- **Opponent / You toggle** — switch between tracking the opponent's accuracy or your own. Full move history is kept for both sides so switching is instant
- **Stats** — moves analyzed, average CPL, best move rate, Lichess-style accuracy
- **Suspicion gauge** — multi-factor score adjusted for ELO: Likely clean → Somewhat suspicious → Highly suspicious → Engine detected
- **Re-analyze game** — replays all recorded moves through the engine from the start, recalculating every CPL value. Use after switching target or if stats look off
- **Reset button** — clear stats for a new game

### Skins Tab

- **Custom piece skins** — click any piece tile (white/black king, queen, rook, bishop, knight, pawn) to upload a custom image. Click the × to remove
- **Board BG** — upload a custom board background image
- **Light/Dark tiles** — upload separate images for light and dark squares. Uses a `repeating-conic-gradient` CSS mask to apply the image only to the correct squares
- **Piece mask mode** — toggle to use the original piece shape as a mask filled with your custom image. For tiles: each square gets the image individually (12.5% 12.5%) instead of one large image showing through (cover)
- **Reset all skins** — clear all custom skins at once
- All skins persist in localStorage across sessions

## Mobile / iOS Safari (chess-assist-mobile.js)

Same features as desktop (including skins, live win% eval bar and engine detection) except no keybind system and no hide-all toggle (mobile doesn't use keyboards). Touch-optimized with larger targets (48px bubble, 28px switches, 22px slider thumbs), close button on panel, slightly longer timeouts for slower devices.

### Usage — Loader Bookmarklet

Always runs the latest version from GitHub:

1. Create a bookmark in Safari with this URL:
   ```
   javascript:void(fetch('https://raw.githubusercontent.com/ThatKidGlitchy/chess-hacks/main/chess-assist-mobile.js').then(r=>r.text()).then(eval))
   ```
2. Open a chess.com game in Safari, tap the bookmarklet
3. Tap the knight icon to open the panel

### Alternative — paste in console

On desktop Safari or any browser with console access, paste the contents of `chess-assist-mobile.js` directly.

## Engine

Both versions use Stockfish 19 WASM (lite single-threaded build) loaded from unpkg CDN. Falls back to ASM.js if WASM is unavailable. MultiPV 3 with anti-repetition logic to avoid draw loops.

## Arrow Colors

| Arrow | Color | Meaning |
|-------|-------|---------|
| Green | #96bc4b | Your best move |
| Red | #e04040 | Opponent's best reply |

## Notes

- Works on chess.com only (relies on chess.com DOM structure)
- The script runs entirely client-side — no data sent anywhere
- Use the loader bookmarklet to always get the latest version automatically
- Both arrow types can be shown simultaneously (different SVG layers)
- Thank you "Chess helper (chess.com)" on the Chrome web store for the UI theme!
