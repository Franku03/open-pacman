# AGENTS.md

## Run

No build step, no npm, no bundler. Open `src/index.html` directly in a browser.

## Architecture

Vanilla JS + HTML + Canvas. **No ES modules** — scripts are loaded as plain `<script>` tags in `src/index.html` and communicate via `window.*` globals. Load order is fixed in index.html and matters:

1. `maze.js` — maze data. Exposes `MAZE`, `TUNNEL_ROW`, `PACMAN_START`, `GHOST_STARTS`.
2. `game.js` — state and rules. Depends on maze.js globals. Exposes `createGame`, `update`, `DIRS`.
3. `render.js` — canvas drawing. Exposes `draw`. Reads `game.grid` (a per-game copy), not `MAZE`.
4. `main.js` — entry point: game loop, keyboard, overlay.

Adding a file = add a `<script>` tag to index.html. Do not use `import`/`export`.

### Maze encoding (`maze.js`)

Grid is 31 strings of 28 chars, parsed to numbers. Legend: `#`=wall(1), `.`=dot(2), ` `=empty(0), `-`=pen door(3). Coordinates `(x,y)`, origin top-left, x∈[0,27] y∈[0,30]. Symmetric around the vertical axis between cols 13-14. `MAZE` stays pristine; each `createGame()` copies it into `game.grid` — mutate the copy, never `MAZE`.

## Spec-driven workflow

This project practices spec-driven development (skills `spec` / `spec-impl`).

- `/spec <description>` — designs a spec via clarifying questions, saves to `specs/NN-slug.md` as `Draft`. **Writes no code.**
- Mark the spec `Approved` manually when ready.
- `/spec-impl NN-slug` — implements an approved spec step by step on branch `spec-NN-slug`. Only proceeds when the spec's state means "Approved".

## Conventions

- Code in English; comments in Spanish only when they add value.
- 2-space indent, single quotes, spaces inside parens in calls/conditionals: `foo( x )`, `if ( c )`.
- No tests, lint, or typecheck configured — verify changes by running the game in a browser.
