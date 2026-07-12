# GomukuLite.AI

A lightweight standalone offline Gomoku game, AI opponent, tactics academy and local coach.

This repository is the **Lite** build derived from the GomukuMaster.AI standalone concept. It is intentionally simple: one self-contained HTML application that can run directly from disk without a build chain, package manager, service worker, WebSocket server or internet connection.

## Run it

Download or clone the repository, then open:

```text
index.html
```

That is all. It works from:

- a local folder
- a USB drive
- GitHub Pages
- any static file server
- locked-down computers where Node/npm cannot be installed

## Included features

- 15×15 Gomoku board
- Freestyle rule: five or more contiguous stones wins
- Standard rule: exactly five contiguous stones wins
- AI levels: Novice, Beginner, Intermediate, Advanced, Expert and Master
- Heuristic alpha-beta AI with candidate ordering and a small transposition cache
- Local two-player mode
- Master hint
- Undo
- Move log
- 108 generated Academy tactics
- Lightweight AI Coach profile
- JSON export/import for game and profile state
- Keyboard-accessible board controls
- Visible coordinate markings outside the board: columns A–O and rows 15–1
- AI move commentary that explains wins, blocks, threats and follow-up candidates after each move
- Board-centred exclamation declaration overlay:
  - AI mode, human wins: **Victory! You win.**
  - AI mode, AI wins: **Defeat. The AI wins.**
  - Local two-player mode: **Black wins!** or **White wins!**
  - Full board: **Draw game.**
- Tutorial/Academy correct and missed attempts now trigger highlight, declaration and jingle feedback
- Glowing highlight on the winning stone line or tutorial solution
- More reliable offline Web Audio win/lose/draw jingles with a mute option
- No external scripts, fonts, images, telemetry, CDN calls or server calls

## Why this exists

The full GomukuMaster.AI repository is a richer React/Vite PWA with a service worker, app manifest, academy, coach and optional WebSocket rooms. That is powerful, but an offline-first PWA and a true standalone file solve different deployment problems.

A PWA depends on a browser-managed first-load cache and served assets. GomukuLite.AI avoids that dependency. The entire app is inside `index.html`, so it can be copied, emailed, archived, opened from `file://`, or distributed on a USB drive.

## Repository structure

```text
.
├── index.html                       # single-file standalone app
├── README.md                        # project overview
├── .nojekyll                        # GitHub Pages static-file compatibility
├── LICENSE                          # MIT licence
├── docs/
│   └── BUILD_REVIEW.md              # implementation rationale and review notes
└── .github/workflows/
    └── pages.yml                    # optional static GitHub Pages deployment
```

## Design principles

1. **Offline means direct-open capable.** No server or cache lifecycle is required.
2. **Lite means no dependency stack.** Everything needed to run is in one file.
3. **Useful AI beats cosmetic difficulty labels.** Each level changes search depth, candidate width, blunder probability and noise.
4. **Analysis should replace passive notes.** The sidebar now provides tactical commentary instead of static outcome reminders.
5. **Training matters.** The Academy teaches patterns and now gives tutorial-style result feedback.
6. **Coordinates should aid play without cluttering intersections.** A–O and 15–1 markings sit outside the playable grid.
7. **Feedback matters.** Completed games give clear visual, textual and audio closure with exact outcome wording.
8. **Private by default.** Profile data remains in the browser unless exported by the user.

## Limitations

This Lite build deliberately removes:

- online rooms
- multiplayer server authority
- React/Vite build tooling
- service-worker install lifecycle
- cloud sync
- external analytics
- tournament-grade Renju forbidden-move validation

For the full production training system, use GomukuMaster.AI. For a portable, no-setup offline version, use GomukuLite.AI.

## Deployment

The included GitHub Actions workflow publishes the repository as a static GitHub Pages site when changes land on `main`. You can also ignore Pages completely and open `index.html` directly.

## Licence

MIT — see [`LICENSE`](LICENSE).
