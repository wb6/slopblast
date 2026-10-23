# Slop Blast

A mobile-first, installable block-blasting puzzle game that works offline.

## How to play

- Drag pieces from the bottom tray onto the 8×8 grid.
- Fill a complete row, column, or long diagonal to clear it and score points.
- **Easy mode**: free line shifting, mystery color bombs, skip pieces, and pushable lines.
- **Medium mode**: line shifting costs points; mystery emojis appear every few moves.
- **Hard mode**: nuisance emojis block lines and scoring gets tougher.
- Double-tap the mode pill in the top bar to cycle modes.

## Controls

| Action                               | How                               |
| ------------------------------------ | --------------------------------- |
| Move piece                           | Drag from tray to grid            |
| Rotate                               | Not needed — pieces are pre-rotated |
| Shift a row/column                   | Press-and-hold grid, then swipe |
| Push a row/column (Easy only)        | Press-and-hold grid for 5 s, then swipe |
| Use mystery color bomb (Easy only)   | Drag the mystery emoji onto a block |
| Skip a piece (Easy only)             | Triple-tap the piece in the tray (−10 pts) |
| Open options                         | Tap **Options**                   |

## Offline & install

Serve the folder over `localhost`/HTTPS and add it to your home screen. Opening the file directly (`file://`) will not enable offline caching because the Service Worker needs a real origin.

```bash
python3 -m http.server 8000
```

## Tech

- Single-file vanilla HTML/CSS/JS game logic.
- PWA manifest and Service Worker for offline use.
- No external libraries, no audio, haptic-only feedback.

## Building

To embed `README.md` into the in-game About screen, run:

```bash
python3 build.py
```

## License

Made for fun. No external assets are used.

## Full game rules and features

### Core mechanics (all modes)

- The board is an 8×8 grid.
- Drag a piece from one of the 3 slots in the bottom tray and drop it onto an empty area on the grid.
- Pieces cannot be rotated — each rotation exists as its own selectable piece.
- A line clears when every cell in a **row**, **column**, or either long **diagonal** is filled.
- Each cleared cell scores **10 points**. The popup counts up as it is added to your total.
- There is no audio; feedback uses short haptic taps.
- The game serves from one HTML file, a Web App Manifest, and a Service Worker for offline play.

### Easy mode

- **Goal:** relaxed play. The game never forces a game over.
- **Line shifting:** press and hold a row/column for about 0.4 s, then swipe left/right or up/down to shift that line. Lines wrap around.
- **Pushable lines:** press and hold a line for **5 seconds**. The highlight turns orange. Swipe to push all blocks on the line together toward one edge (like a pusher game). No wrapping.
- **Mystery emoji:** a positive mystery emoji can appear on the board. Drag it onto any block to blow up all connected blocks of the same color for **15 points each**.
- **Skip piece:** triple-tap a slot to discard its current piece and draw another. Costs **10 points** and your score may go negative.
- **Mystery spawn:** controlled by a slider in Options (1%–100% per move). Default is 80%.
- Life spans: each mystery lasts **3 moves** before it disappears and respawns.

### Medium mode

- **Goal:** balance strategy and pressure.
- **Line shifting:** costs **100 points** each shift.
- **Mystery emoji:** a positive reward emoji may appear. Clearing the line that contains it multiplies that clear by 2×–5× (weighted toward lower values).
- **Spawn rule:** a new mystery is only rolled on every **3rd move**, with a **20%** chance.
- **Game over:** when none of the 3 current pieces can legally be placed. Top scores store whether mystery was on or off.

### Hard mode

- **Goal:** survive the nuisance.
- **Nuisance emoji:** spawns frequently as a negative emoji (e.g., angry face, poop, stop sign). It occupies a cell and blocks any line that passes through it from clearing.
- **Line shifting:** costs **half your current score**, with a minimum of **100 points**.
- **Swapping into Hard:** resets your score to **0** but keeps the current board and refreshes your hand.
- **Game over:** when none of the 3 pieces can be placed. Managing nuisance lines is the key difficulty.

### Modes quick-switch

Double-tap the mode pill in the top-right of the score bar to cycle Easy → Medium → Hard → Easy. A small pulse confirms the change.
