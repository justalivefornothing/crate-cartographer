# Crate Cartographer — Plan

A sokoban rebuild with unlimited undo, a paint-style level editor,
corner-deadlock warnings, and shareable level codes.

## Goal

Build a small, polished sokoban that feels like a drafting table: navy
blueprint background, cyan hairline grid, cross-hatched walls, isometric
crates. The hook: paint a level, hit Play, push a crate into a corner and the
game outlines it red with a "deadlock" badge before you waste twenty moves.

## Features (all required)

1. Standard sokoban rules — push one crate, no pulls, win when every crate sits on a goal.
2. Undo/redo with full history plus a move/push counter.
3. Ten built-in levels parsed from the standard text format (`#`, `@`, `$`, `.`, `*`, `+`).
4. Level editor: tile palette, drag painting, resize, validation (exactly one player, crates == goals).
5. Static corner-deadlock detection — highlight crates that can never reach a goal.
6. Export/import levels as a compressed base64 code in the URL hash.
7. Keyboard (arrows / WASD / Z / Y / R) and on-screen d-pad controls.

## Architecture

```
src/
  core/
    level.ts       parse / serialize the text format, tile helpers
    game.ts        immutable GameState, move(), isSolved(), history helpers
    deadlock.ts    deadSquares(): reverse-pull flood fill from each goal
    codec.ts       level <-> compact base64url code (RLE + base64)
    levels.ts      10 built-in levels
    *.test.ts      vitest unit tests (spec assertions at minimum)
  ui/
    Board.tsx      SVG board: grid paper, hatched walls, isometric crates, deadlock badges
    Editor.tsx     drafting toolbar palette, drag painting, resize, validation
    DPad.tsx       on-screen controls
    Hud.tsx        move/push counters, undo/redo, level picker, share
  App.tsx          mode switch (play / edit), keyboard handling, URL hash sync
```

Core is pure TypeScript with no React imports so it can be unit-tested in the
node environment. State is an immutable snapshot; the history is an array of
snapshots plus a cursor, so undo/redo are O(1) pointer moves.

## Core algorithm

- **Parser**: split lines, map each char to wall/floor/goal flags, extract
  player position and crate set. Unknown chars are treated as floor.
- **Move resolution**: compute target cell; wall -> reject (return same
  reference). Crate at target -> compute push target; wall or crate -> reject;
  otherwise move crate and player. Rejected moves add no history entry.
- **Dead squares**: for each goal, flood fill backwards. A crate can be
  *pulled* from square S to neighbour N (direction d) only if N and N+d are
  both non-wall. Any floor square never reached by a reverse pull from any goal
  is dead: a crate there can never reach a goal. Crates on dead squares get a
  red outline and a "deadlock" badge.

## Milestones

1. chore: plan, license, gitignore
2. chore: scaffold vite-react + tailwind + vitest
3. feat: core (parser, moves, undo history, dead squares, codec) + tests
4. feat: blueprint board + play mode + keyboard/d-pad + HUD
5. feat: level editor with palette, drag painting, resize, validation
6. feat: shareable level codes via URL hash
7. docs: readme with screenshot
