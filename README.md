# Crate Cartographer

Sokoban rebuild with unlimited undo, a paint-style level editor, corner-deadlock warnings, and shareable level codes.

## Features

- Classic rules (push only; win when all crates are on goals)
- Full undo / redo + move/push counters
- Built-in levels in standard text format
- Level editor: tile palette, drag painting, resize, validation
- Static corner-deadlock detection (outline + badge)
- Export / import levels as compact base64 codes in the URL hash
- Keyboard (arrows / WASD / Z / Y / R) + on-screen D-pad

## Tech

TypeScript core (parser, game state, deadlock flood-fill, codec) + React UI.

## Run

```bash
npm install
npm run dev
```

## License

MIT
