# p5.js Viz Template

Single-file starter for 2D scientific visualization. Copy `index.html`, add your sketch, open in browser — no build step.

![](docs/screenshot.png)

## What's included

- Pan (drag) and zoom (scroll / pinch) with world-coordinate helpers
- Auto-generated parameter sidebar from a plain JS object
- Touch support: single-finger pan, pinch zoom, 🖐/✏️ mode toggle for mobile interaction
- Dark theme, responsive layout, FPS display

## Start a new project

```
cp index.html my-project/
```

Edit three sections inside `<script>`:

| Section | What to put there |
|---|---|
| `① PARAMS` | Your sliders, checkboxes, color pickers |
| `② USER SETUP` | One-time init (load data, create objects) |
| `③ USER DRAW(t)` | Per-frame drawing — `t` is elapsed seconds |

Everything else is infrastructure — leave it alone.

## Docs

- **For AI agents** → `CLAUDE.md` (full API reference, auto-read by Claude Code)
- **In-file** → header block at top of `<script>` lists what's provided
