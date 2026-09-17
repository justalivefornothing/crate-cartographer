# Crate Cartographer

A Sokoban rebuild with unlimited undo, a paint-style level editor, corner-deadlock warnings, and shareable level codes.

## Features

- Classic Sokoban rules (push only, win when all crates on goals)
- Full undo / redo + move/push counters
- 10 built-in levels (standard text format)
- Level editor: tile palette, drag painting, resize, validation
- Static corner-deadlock detection (red outline + badge)
- Export / import levels as compact base64 codes in the URL hash
- Keyboard (arrows / WASD / Z / Y / R) + on-screen D-pad

## Tech

Pure TypeScript core (parser, game state, deadlock flood-fill, codec) + React UI with blueprint aesthetic.

## Status

See `PLAN.md` for architecture and milestones.

## License

MIT
