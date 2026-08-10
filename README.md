# Koch Snowflake

A single-file, interactive snowfall animation built with HTML5 Canvas. Each snowflake is procedurally generated using a randomized Koch curve with six-fold symmetry, pre-rendered to offscreen bitmaps, and animated as it drifts and sways across the sky.

**Day** and **Night** themes switch the background, distant snow, flake palette, and UI chrome. The chosen mode is saved to `localStorage`.

## Quick start

Open [Falling Snowflakes](https://tengyanhaiin-star.github.io/Falling-Snowflakes/) in any modern browser — no build step or server required.

## Preview

The scene layers three elements:

- **Foreground snowflakes** — detailed Koch crystals with filled arms, hexagonal rings, and per-flake gradients
- **Distant snow** — small, semi-transparent particles that fall more slowly to suggest depth
- **Sky background** — a radial gradient drawn on the canvas; the page body uses a solid theme color so mobile browser chrome (Safari toolbars) can pick it up

### Themes

| | **Day** (default) | **Night** |
|---|-------------------|-----------|
| Sky | Slate blue-gray (`#707f8f`) | Deep navy (`#060d18`) |
| Flakes | White / off-white | Light blue-white |
| UI | Light panel, dark text | Dark panel, light text |

## Controls

| Control | Range | Default | Description |
|---------|-------|---------|-------------|
| **Snowflake count** | 4 – 28 | 12 | Number of foreground Koch snowflakes on screen |
| **Fall speed** | 0.4× – 2.0× | 1.0× | Global speed multiplier for all falling particles |
| **Scene mode** | Day / Night | Day | Switches theme; persisted in `localStorage` under `snowflake-scene-mode` |

**Tap or click the canvas** to regenerate every foreground snowflake and all distant-snow particles with new random shapes and positions.

When a snowflake drifts off the bottom of the screen, it respawns at the top with a freshly randomized shape.

## How the snowflakes are built

Each crystal is generated in four stages:

1. **Random Koch arm** — A stochastic Koch subdivision runs along a half-arm on the +x axis. At each of four depth levels (`KOCH_DEPTH = 4`), branches may fork outward based on per-flake random parameters (`bumpChance`, `bumpScale`).
2. **Mirror symmetry** — The half-arm is mirrored across the x-axis so each main arm is bilaterally symmetric.
3. **Six-fold rotation** — The mirrored arm is copied six times at 60° intervals (D6 symmetry).
4. **Center rings** — Two flat-top hexagonal rings are drawn near the hub to form a classic snowflake core.

Fill and stroke paths are built separately. Generation guarantees visible structure: at least one depth-4 bump on fill or stroke, and a forced depth-3 bump on the outermost spine segment of the stroke path.

Every snowflake receives a unique 32-bit seed. Seeds are deduplicated within each batch so no two flakes on screen share the same shape at generation time.

Visual properties (hue, lightness, opacity, line weight, size, fall speed, sway) are randomized per flake within the active theme's palette. Shapes are baked to offscreen bitmaps once; the animation loop only needs `drawImage` calls.

## Performance

- Geometry and bitmaps are baked once per snowflake; the render loop clears the canvas, draws the background gradient, updates particle positions, and blits cached images.
- Device pixel ratio is capped at 2× for sharp rendering on Retina displays without excessive memory use.
- Theme switches re-tint and re-bake existing flake bitmaps in place without a full scene rebuild.

## Mobile support

The page is tuned for iPhone and iOS Safari:

- Pinch-to-zoom is disabled across the page (sliders still respond to single-finger drag)
- Safe-area insets keep controls clear of the home indicator and notch
- UI chrome tracks the visual viewport so controls stay anchored to the bottom of the visible area
- Landscape mode switches to a compact single-row control bar and hides the hint text
- Custom-styled mode dropdown with theme-aware arrow (no native iOS select chrome clipping)
- `theme-color` meta tag and `body.style.background` update on theme change for browser UI sampling
- Tap the canvas to regenerate (no 300 ms click delay)

## Dependencies

- [Roboto](https://fonts.google.com/specimen/Roboto) via Google Fonts (loaded at runtime; requires network on first visit)

Everything else is vanilla HTML/CSS/JavaScript with no frameworks.

## Browser compatibility

Works in current versions of Chrome, Firefox, Safari, and Edge. Requires Canvas 2D and `requestAnimationFrame`.

## License

MIT — see [LICENSE](LICENSE) for details.
