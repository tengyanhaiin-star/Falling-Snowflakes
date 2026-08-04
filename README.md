# Koch Snowflake

A single-file, interactive snowfall animation built with HTML5 Canvas. Each snowflake is procedurally generated using a randomized Koch curve with six-fold symmetry, then rendered as a glowing ice crystal that drifts and sways across a dark winter sky.

Open `snowflake.html` in any modern browser — no build step or server required.

## Preview

The scene layers three elements:

- **Foreground snowflakes** — detailed Koch crystals with filled arms, hexagonal rings, and soft gradients
- **Distant snow** — small, semi-transparent particles that fall more slowly to suggest depth
- **Night sky** — a radial gradient background with a subtle starfield effect

## Controls

| Control | Range | Default | Description |
|---------|-------|---------|-------------|
| **Snowflake count** | 4 – 28 | 12 | Number of foreground Koch snowflakes on screen |
| **Fall speed** | 0.4× – 2.0× | 1.0× | Global speed multiplier for all falling particles |

**Tap or click the canvas** to regenerate every foreground snowflake and all distant-snow particles with new random shapes and positions.

When a snowflake drifts off the bottom of the screen, it respawns at the top with a freshly randomized shape.

## How the snowflakes are built

Each crystal is generated in four stages:

1. **Random Koch arm** — A stochastic Koch subdivision runs along a half-arm on the +x axis. At each of four depth levels, branches may fork outward based on per-flake random parameters (`bumpChance`, `bumpScale`).
2. **Mirror symmetry** — The half-arm is mirrored across the x-axis so each main arm is bilaterally symmetric.
3. **Six-fold rotation** — The mirrored arm is copied six times at 60° intervals (D6 symmetry).
4. **Center rings** — Two flat-top hexagonal rings are drawn near the hub to form a classic snowflake core.

Every snowflake receives a unique 32-bit seed. Seeds are deduplicated within each batch so no two flakes on screen share the same shape at generation time.

Visual properties (hue, lightness, opacity, line weight, size, fall speed, sway) are randomized per flake. Shapes are pre-rendered to offscreen bitmaps so the animation loop only needs `drawImage` calls.

## Performance

- Geometry and bitmaps are baked once per snowflake; the render loop clears the canvas, draws the background gradient, updates particle positions, and blits cached images.
- Device pixel ratio is capped at 2× for sharp rendering on Retina displays without excessive memory use.

## Mobile support

The page is tuned for iPhone and iOS Safari:

- Pinch-to-zoom is disabled across the page (sliders still respond to single-finger drag)
- Safe-area insets keep controls clear of the home indicator and notch
- UI chrome tracks the visual viewport so controls stay anchored to the bottom of the visible area
- Landscape mode switches to a compact single-row control bar and hides the hint text
- Tap the canvas to regenerate (no 300 ms click delay)

## Dependencies

- [Roboto](https://fonts.google.com/specimen/Roboto) via Google Fonts (loaded at runtime; requires network on first visit)

Everything else is vanilla HTML/CSS/JavaScript with no frameworks.

## Browser compatibility

Works in current versions of Chrome, Firefox, Safari, and Edge. Requires Canvas 2D and `requestAnimationFrame`.

## License

MIT — see [LICENSE](LICENSE) for details.
