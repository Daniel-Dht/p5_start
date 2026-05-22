# p5.js Scientific Visualization Template

A reusable single-file starter for 2D scientific visualization projects.
Copy `index.html` to begin any new project — pan/zoom, parameter sidebar,
touch support, and coordinate helpers are already built in. Do not reimplement them.

---

## The three sections you edit

Everything else in `index.html` is infrastructure. Only touch these three:

| Section | Where | Purpose |
|---|---|---|
| `① PARAMS` | top of `<script>` | Declare parameters — sidebar UI is auto-generated |
| `② USER SETUP` | after PARAMS | One-time init (load data, create objects…) |
| `③ USER DRAW(t)` | after USER SETUP | Per-frame drawing in world coordinates |

---

## PARAMS — the key pattern

```js
const PARAMS = {
  myFlag:  { value: true,   type: 'checkbox', label: 'My flag',  group: 'Section A' },
  mySpeed: { value: 1.0,    type: 'slider',   label: 'Speed',    min: 0, max: 5, step: 0.1, group: 'Section A' },
  myCount: { value: 3,      type: 'number',   label: 'Count',    min: 1, max: 20, step: 1,  group: 'Section B' },
  myColor: { value: '#4af', type: 'color',    label: 'Color',    group: 'Section B' },
};
```

- **Read anywhere**: `PARAMS.mySpeed.value`
- **Write**: `PARAMS.mySpeed.value = 2.5` (DOM syncs automatically)
- **Types**: `checkbox` | `slider` (needs min/max/step) | `number` (needs min/max/step) | `color`
- `group` is optional — inserts a labelled section header in the sidebar
- "Reset defaults" button in the sidebar restores all initial values

---

## userDraw(t) conventions

```js
function userDraw(t) {
  // t = elapsed seconds — use for animation: sin(t * PARAMS.speed.value)
  // Camera transform already applied — draw in world coordinates directly
  // World origin (0,0) = canvas centre; x→right, y→down (p5 default)
  // push()/pop() nest correctly inside here
  // Scale stroke weights with 1/cam.zoom to keep them visually constant
  strokeWeight(2 / cam.zoom);
}
```

---

## Available APIs — do not reimplement

### Camera
| Symbol | Type | Description |
|---|---|---|
| `cam.x`, `cam.y` | number | Pan offset in screen pixels (read/write) |
| `cam.zoom` | number | Zoom scale factor (read/write) |
| `screenToWorld(sx, sy)` | `→ {x,y}` | Screen px → world coords (use for mouse hit-tests) |
| `worldToScreen(wx, wy)` | `→ {x,y}` | World coords → screen px |

### Built-in infrastructure
- **Pan**: mouse drag, single-finger touch
- **Zoom**: scroll wheel + pinch; zooms toward cursor/midpoint; clamped [0.01, 200]
- **Grid**: `PARAMS.showGrid.value` toggles; `PARAMS.gridStep.value` sets spacing
- **Axes**: `PARAMS.showAxes.value` toggles X/Y axis lines through origin
- **FPS bar**: rolling 30-frame average, updates every 15 frames, shown in sidebar footer
- **Sidebar**: auto-generated from PARAMS; collapses with ⚙ button
- **Mobile mode toggle**: 🖐 (pan) / ✏️ (edit) button, bottom-right, visible on screens ≤600px wide

### Touch edit mode
In ✏️ edit mode, single touches hit-test and drag scene objects instead of panning.
The hit-test radius is `18 / cam.zoom`. To add new draggable objects:
1. Extend the hit-test loop in `touchStarted` (mirrors `mousePressed` logic)
2. Extend the drag logic in `touchMoved` (mirrors `mouseDragged` logic)

Pinch-zoom always works regardless of mode.

### Mouse interaction
`mousePressed` / `mouseDragged` already handle:
- Left-drag on empty canvas → pan
- Left-drag on a bézier control point → move that point
Hit radius: `14 / cam.zoom`. Add more hit-testable objects in the same pattern.

---

## Build info update (do on every commit)

```bash
# After git commit:
git rev-parse --short HEAD     # → e.g. f45353f
git log -1 --format="%ci"     # → e.g. 2026-05-22 08:42:00
```

Update the two constants near the bottom of the `<script>` block in `index.html`:

```js
const _BUILD_HASH = 'f45353f';
const _BUILD_DATE = '2026-05-22 08:42';
```

Then make a second commit: `chore: set build hash to <hash>`.

---

## Starting a new project from this template

1. Copy `index.html` to a new folder
2. Replace `① PARAMS` with your project's parameters
3. Replace `② USER SETUP` and `③ USER DRAW` with your sketch
4. Leave everything below the `p5 CORE` banner untouched
5. Update `_BUILD_HASH` / `_BUILD_DATE` on each commit

---

## File structure

```
index.html          — entire app (HTML + CSS + JS, no build step)
CLAUDE.md           — this file
```

`index.html` internal sections (in order):
```
<style>             CSS (theme variables, layout, sidebar, mobile)
① PARAMS            ← edit
② USER SETUP        ← edit
③ USER DRAW         ← edit
p5 CORE             infrastructure (camera, grid, mouse, touch, resize)
UI BUILDER          sidebar DOM generation, reset, mode toggle, build bar
```
