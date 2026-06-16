# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

No build step. Open directly or via a local static server:

```bash
open index.html                 # macOS — open in default browser
python3 -m http.server 8000    # then visit http://localhost:8000
npx serve .
```

## Architecture

Three files, no dependencies, no bundler:

- **`index.html`** — DOM structure: `<canvas id="board">` (300×600 px, 10×20 grid at 30 px/block) and `<canvas id="next-canvas">` (120×120 px preview). A single `#overlay` div handles both PAUSE and GAME OVER states by toggling `.hidden`.
- **`style.css`** — Dark/retro theme using CSS variables, flexbox, and `backdrop-filter` on the overlay.
- **`game.js`** — All game logic (~305 lines, `'use strict'`, no modules).

### Key data structures in `game.js`

- **`board`**: `ROWS × COLS` (20×10) 2-D array where `0` = empty, `1–7` = piece color index.
- **`current` / `next`**: `{ type, shape, x, y }` — `shape` is a square matrix of color indices.
- **`PIECES`**: Indexed 1–7 (index 0 is `null`). Each piece stores its color index directly in the matrix cells.

### Game loop

`init()` → `spawn()` → `requestAnimationFrame(loop)`. The loop accumulates delta time in `dropAccum`; when it exceeds `dropInterval` the piece drops one row or locks. Rendering (`draw()`) happens every frame.

### Critical relationships

- **Piece locking**: `lockPiece()` calls `merge()` → `clearLines()` → `spawn()`. If `spawn()` detects a collision at spawn position, it calls `endGame()`.
- **Speed formula**: `dropInterval = Math.max(100, 1000 − (level − 1) × 90)` ms. Level increases every 10 lines.
- **Rotation**: `rotateCW()` does transpose + reverse. `tryRotate()` applies wall kicks `[0, -1, 1, -2, 2]` columns.
- **Ghost piece**: computed live each frame via `ghostY()` and drawn at `globalAlpha = 0.2`.
- **Scoring**: `LINE_SCORES = [0, 100, 300, 500, 800]` × level; hard drop +2 pts/row, soft drop +1 pt/row.

### Canvas dimensions

If `COLS`, `ROWS`, or `BLOCK` constants change, the `width`/`height` attributes on `<canvas id="board">` in `index.html` must be updated to `COLS × BLOCK` and `ROWS × BLOCK` respectively.
