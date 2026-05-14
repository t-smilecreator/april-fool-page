# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a static HTML project with no build system, package manager, or server. All files are self-contained and can be opened directly in a browser. Content is written in Japanese.

## Files

- **`index.html`** — An Amazon.co.jp-styled April Fool's prank page. It mimics Amazon's UI (dark header `#131921`, yellow `#ffd814` buttons) and simulates a lottery where a user "wins" ¥100,000 in gift cards. After clicking the proceed button, it reveals the joke. Uses two local JPG images (`91376f42-...jpg` for the "win" screen, `c6a344eb-...jpg` for the reveal).
- **`az104-quiz.html`** — A standalone AZ-104 Azure Administrator exam quiz in Japanese. Contains all 44 questions, CSS, and JavaScript inline. No external dependencies.

## Development

No build step. Open HTML files directly in a browser:

```
open index.html
open az104-quiz.html
```

There are no tests, linters, or package dependencies in this project.

## Architecture: az104-quiz.html

All logic is in a single file. Key structure:

- **`questions[]`** — Array of objects with fields: `cat` (category string), `q` (question text), `opts` (answer array), `ans` (zero-based correct index), `exp` (HTML explanation string).
- **Categories**: `"identity"`, `"storage"`, `"compute"`, `"network"`, `"monitor"`
- **`answered{}`** — Object keyed by global question index tracking the user's selected option.
- **`correct_count` / `answered_count`** — Global score counters updated by `check()`.
- **`render()`** — Re-renders the quiz DOM from `filtered()` (respects active category). Called on filter button clicks and on initial load.
- **`select(gidx, oi)`** — Marks a pending selection on a card (stores chosen index on the button element via `._sel`).
- **`check(gidx)`** — Commits the answer: updates `answered{}`, increments counters, applies `.right`/`.wrong` CSS classes, shows explanation, disables the button.
- **`updateStats()`** — Refreshes the score bar and header stats display.

To add questions, append objects to the `questions[]` array using the same schema. The `q-num` label uses the global index (`gidx + 1`), so insertion order matters.

## Architecture: index.html

- **`startLottery()`** — Reads name input, shows a loading state for 3 seconds via `setTimeout`, then renders a "winner" message with the first image.
- **`reveal()`** — Replaces the result div with the April Fool's message and second image.
- Images are referenced by their UUID filenames; keep those files alongside `index.html`.
