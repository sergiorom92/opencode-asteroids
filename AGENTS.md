# AGENTS.md

Zero-dependency Asteroids clone. All game logic lives in `game.js` (plain `<script>`, no modules/exports); `index.html` is just an 800x600 canvas shell.

## Run / verify

- Play: open `index.html` directly or `npx serve .`.
- No build, tests, lint, or CI. Syntax check: `node --check game.js`.
- Headless logic check: `game.js` touches `document`/`window` at load, so stub `document.getElementById`, `window.addEventListener`, `requestAnimationFrame`. `eval` the source after replacing the trailing `initGame();\nrequestAnimationFrame(loop);` block (note: the string `initGame();` also appears inside `update()` — don't replace the first occurrence).

## Conventions

- Fixed world `W=800, H=600` with toroidal `wrap()`; every entity update must wrap.
- Input uses `e.code` via `keys` (held) + `justPressed`/`pressed()` (single-fire, e.g. `Space`).
- UI text is Spanish (`NIVEL`, `PUNTAJE`, `VELOCIDAD`) — keep new strings Spanish.
- Game flow states: `playing` | `dead` (2s timer, then `ship.reset()`) | `gameover` (`Space` restarts). Pause boost/powerup timers by only ticking them in `playing`.
- Features: ship (`THRUST`/`DRAG`/`ROT`), bullets (`ttl`), splitting asteroids (`RADII`/`SPEEDS`/`POINTS` by size 3/2/1), explosion `particles`, `velocidad` pickup (cyan `V` diamond, `BOOST_TIME=5`, `BOOST_MULT=2`, re-collect refreshes; spawner: first ~8s, then every 12s if field empty; cleared on `initGame()`/`nextLevel()`).
