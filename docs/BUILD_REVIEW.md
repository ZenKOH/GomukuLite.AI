# GomukuLite.AI Build Review

## Objective

Create a standalone offline Gomoku build based on the GomukuMaster.AI offline-edition design.

The target build is not a smaller React/Vite app. It is a direct-open, single-file HTML application for low-connectivity, workshop, classroom, travel and locked-down-computer use cases.

## Key product distinction

An offline-first PWA is not the same as a standalone offline file.

A PWA usually depends on:

- a served asset path
- an app manifest
- browser-managed service-worker lifecycle
- a first successful online load before offline caching becomes reliable
- generated build assets

GomukuLite.AI avoids those assumptions by putting CSS, UI, game logic, AI, Academy generation and profile handling inside `index.html`.

## What was implemented

- Single-file `index.html`
- 15×15 responsive board
- Freestyle and Standard rules
- AI ladder from Novice to Master
- Candidate generation near occupied stones
- Heuristic evaluation across rows, columns and diagonals
- Alpha-beta minimax for stronger levels
- Small transposition cache inside each AI turn
- Two-player local mode
- Hint system
- Undo
- Move log
- 108 generated Academy positions
- Local Coach profile
- JSON export/import
- Keyboard-accessible board
- Zero external assets

## End-game feedback upgrade

The latest build adds a clearer end-game moment when a match completes:

- winning-line detection now returns the exact contiguous line that satisfied the selected ruleset;
- the winning stones receive a pulsing glow animation;
- the side panel shows a declaration such as `Victory! You win.`, `Defeat. The AI wins.`, `Black wins!`, `White wins!` or `Draw game.`;
- Web Audio jingles play locally for win, loss and draw outcomes;
- a Jingle selector lets the player mute sounds without affecting the offline build;
- reduced-motion users still see the static highlight without animation.

The audio is generated with browser oscillators. There are no sound files, CDN calls or external media assets.

## Why the implementation is intentionally constrained

The purpose of GomukuLite.AI is reliability and portability, not maximum engine strength. It should work when copied to a USB drive and opened directly.

That means the Lite edition avoids:

- external packages
- Web Workers
- WASM
- CDN fonts
- image assets
- generated bundles
- server-dependent online rooms
- service-worker cache dependencies

## Technical trade-offs

The AI is intentionally heuristic rather than neural. This keeps the build transparent, small and deterministic enough to run in ordinary browsers.

The Academy positions are generated from transformed motifs rather than stored as a large static dataset. This preserves the “108-position” training promise while keeping the file compact.

The local coach is deliberately lightweight. It uses games, puzzle attempts, hints and simple play-style signals rather than pretending to infer a sophisticated psychological profile from sparse data.

The end-game jingle uses Web Audio rather than bundled audio files to preserve the one-file offline guarantee. Some browsers may require the user to interact with the page before audio can play; the game resumes/initialises audio during player actions.

## Future improvements

1. Add a headless-browser smoke test for `index.html`.
2. Add downloadable release assets.
3. Add SGF-style export.
4. Add Renju forbidden-move validation.
5. Add optional Web Worker AI while preserving the no-build fallback.
6. Add more hand-authored tactical motifs.
7. Add offline print mode for classroom worksheets.
8. Add a settings panel for alternate sound themes and colour-blind-safe highlight palettes.
