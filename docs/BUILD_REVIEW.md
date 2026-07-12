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
- Two-player duel mode with explicit Black/White outcome declarations
- Hint system
- Undo
- Move log
- 108 generated Academy positions
- Local Coach profile
- JSON export/import
- Keyboard-accessible board
- Zero external assets

## Visible board coordinate upgrade

The latest build adds visible board coordinates outside the playable grid.

- Columns are labelled A–O above and below the board.
- Rows are labelled 15–1 on both the left and right sides.
- The labels sit outside the intersections so they do not interfere with stone placement, hover states, touch controls or keyboard focus.
- The board's accessible cell labels already include notation; the visible coordinate frame now makes the same notation immediately usable for sighted play and coaching.
- The coordinate frame scales with the board on mobile layouts.

## Move analysis and commentary upgrade

The build removes the static outcome reminder note from the sidebar and replaces it with an AI-style commentary panel.

The commentary is generated locally from the current board state and explains each move in practical tactical language:

- whether the move completed a win or settled a two-player duel;
- whether the next urgent task is defence;
- whether a forcing continuation is available;
- where the strongest follow-up candidate appears;
- how Academy/tutorial attempts relate to the hidden solution.

The commentary remains offline and deterministic. It does not call any external AI service, server or model endpoint.

## Board-centred declaration overlay

Outcome declarations appear over the board itself rather than in the side panel. The declaration is centred near the top of the board and uses an exclamation-style badge for stronger end-game feedback.

Exact wording remains:

- AI mode, human wins: `Victory! You win.`
- AI mode, AI wins: `Defeat. The AI wins.`
- Local two-player mode, Black wins: `Black wins!`
- Local two-player mode, White wins: `White wins!`
- Full board: `Draw game.`

The status pill, board overlay, move log and commentary panel all remain aligned to the same outcome logic. In two-player duel mode, the declaration is always `Black wins!` or `White wins!`, never a generic win/lose message.

## End-game and tutorial feedback upgrade

When a match completes:

- winning-line detection returns the exact contiguous line that satisfied the selected ruleset;
- the winning stones receive a pulsing glow animation;
- the board-centred overlay shows the exact declaration;
- Web Audio jingles play locally for win, loss and draw outcomes;
- a Jingle selector lets the player mute sounds without affecting the offline build;
- reduced-motion users still see the static highlight without animation.

Academy/tutorial attempts now receive the same treatment:

- correct attempts show `Tutorial victory!`, highlight the completed line or solution point, play the win jingle and generate tutorial commentary;
- missed attempts show `Tutorial miss.`, highlight the hidden solution, play the loss jingle and explain why the solution is stronger.

The audio is generated with browser oscillators. There are no sound files, CDN calls or external media assets. The upgraded version now unlocks/resumes Web Audio on pointer or keyboard interaction and schedules the jingle only after the audio context is available, improving reliability on browsers that block sound until user interaction.

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

The end-game jingle uses Web Audio rather than bundled audio files to preserve the one-file offline guarantee. Some browsers may still require the user to interact with the page before audio can play; the game now listens for early pointer and keyboard interaction to unlock sound sooner.

## Future improvements

1. Add a headless-browser smoke test for `index.html`.
2. Add downloadable release assets.
3. Add SGF-style export.
4. Add Renju forbidden-move validation.
5. Add optional Web Worker AI while preserving the no-build fallback.
6. Add more hand-authored tactical motifs.
7. Add offline print mode for classroom worksheets.
8. Add a settings panel for alternate sound themes and colour-blind-safe highlight palettes.
9. Add richer post-game move review with move-by-move mistake classification.
