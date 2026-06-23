# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file habit tracker app (`index.html`) with no build step, no dependencies to install, and no bundler. Open the file directly in a browser or serve it locally to run it.

```bash
# Run locally
open index.html
# or
python3 -m http.server 8080
```

## Architecture

Everything lives in `index.html`. The stack:
- **React 18** via ESM CDN (`esm.sh`) with **htm** for JSX-like templating (no JSX transpilation needed)
- **Tailwind CSS** via CDN
- **localStorage** for persistence (`habit-tracker-habits` key)

### Component tree
```
App
├── Dashboard          — stat cards (total, completed today, best streak)
├── DonutChart         — total all-time time breakdown, groups same-name habits
├── HabitCard[]        — per-habit row with toggle, streak badge, delete
└── AddHabitModal      — form to create a habit (name, time window, color)
```

### Key data shape
```js
{ id, name, color, startTime, endTime, createdAt, completions: ['YYYY-MM-DD', ...] }
```
`color` stores either a palette key (`'teal'`, `'olive'`, etc.) or a raw hex string (`'#a1b2c3'`) for custom picks.

### Color system
`COLOR_OPTIONS` defines the palette (Adobe Color theme + complementary additions). `getHex(key)` resolves both palette keys and direct hex values. `hexToRgba(hex, alpha)` is used for tinted badge backgrounds. No Tailwind color utilities are used for habit colors — all applied via inline styles.

### DonutChart grouping
Same-name habits are merged into one segment. Duration is accumulated as `getDurationMinutes(habit) × completions.length` across all time (not just today).

## Deployment

Hosted on GitHub Pages at `https://cletchy.github.io/hubtrac/`. Push to `main` on `https://github.com/cletchy/hubtrac` to deploy.

## Git workflow

After every meaningful change (feature added, bug fixed, UI updated), commit and push to GitHub:

```bash
git add index.html
git commit -m "short description of change"
git pull origin main --no-rebase
git push
```

The pull before push is required — GitHub Pages sometimes adds commits that need to be merged first.
