# Spline Brushes

`Planet Spline Brush` shapes terrain along a curve on the planet surface. It uses the same height, biome, vertex-color, cutout, falloff, and priority controls as a [Terrain Stamp](terrain-stamps.md).

## Adding a Spline Brush

1. Place a `Planet Spline Brush` actor and assign its `Target Planet`.
2. Edit its `Spline` component to place at least two points along the surface.
3. Set `Radius Cm` for the brush's influence radius, then choose the inherited height operation and any biome or vertex-color effects.

Each spline point's **Scale Y** multiplies `Radius Cm` locally, so the path can widen or narrow. The spline points' radial distance from the planet center supplies the target elevation when `Flatten To Spline` is enabled. In that mode, `Flatten Height` adds an offset to the spline elevation.

`End Falloff Cm` fades an open spline near its ends; closed splines ignore it. `Curve Tolerance Cm` controls how closely the compiled brush follows curved segments. Lower tolerance follows bends more closely and produces more segments.

Spline brushes are editor-authored. The target spawner compiles their effects for runtime and cooked builds. Edits made through the editor update the affected terrain; use `Commit Brush Changes` after Blueprint-authored changes that bypass normal editor notifications.
