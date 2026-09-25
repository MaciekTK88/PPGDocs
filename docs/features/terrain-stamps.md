# Terrain Stamps

`Planet Height Brush` actors provide localized, editor-authored changes to a generated planet. A brush can sculpt terrain, assign biomes, modify material and foliage weights, override vertex-color channels, or remove terrain.

Brush actors are editor-only sources. Each target Planet Spawner stores the compiled brush primitives, texture masks, biome-cell composition, and sparse spatial index required by runtime and cooked builds.

## Adding a Brush

1. Add a `Planet Height Brush` actor to the level.
2. Assign its `Target Planet`.
3. Position the actor on or near the planet surface.
4. Set `Radius`, operation, falloff, and optional texture or biome controls.

The actor's direction from the planet center determines the center of the spherical stamp. Its distance from the center does not set the height value.

## Height Operations

Height operations run after procedural biome-height blending:

| Operation | Behavior |
| --- | --- |
| `Add` | Adds `Height Delta` to the current displacement. |
| `Subtract` | Subtracts the absolute `Height Delta`. |
| `Multiply` | Multiplies the current signed displacement by `Height Multiplier`. |
| `Flatten` | Blends the current displacement toward `Flatten Height`, measured from the base planet radius. |
| `Cutout` | Removes adjoining terrain triangles where influence reaches `Cutout Mask Threshold`. Collision and foliage follow the remaining terrain. |

Disable `Affect Height Displacement` when the brush should affect only biome or vertex-color data. This also disables Cutout.

## Shape, Falloff, and Opacity

`Radius` is measured along the planet surface in centimeters. `Inner Radius Ratio` defines the fully influenced part of the stamp, and `Falloff` controls the transition to zero at the outer radius.

`Brush Opacity` multiplies every enabled effect. A value of `0` disables influence without changing the other brush settings.

## Texture Masks

Assign `Height Texture` to project a scalar mask over the stamp. Select Luminance, Red, Green, Blue, or Alpha with `Texture Channel`, then use `Texture Rotation` and `Texture Scale` to orient the projection.

- white applies full influence
- black preserves the original result
- intermediate values blend proportionally

The same mask can control height, cutout, and enabled vertex-color channels. `Use Texture As Biome Mask` determines whether it also controls biome influence. Inverting a vertex-color channel reverses the texture mask for that channel.

The source texture is compiled at its native size up to the Planet Spawner's `Height Brush Texture Resolution Clamp`. Terrain detail is still limited by the vertex density of the generated chunk.

## Biome Application

Set `Biome Name` to the exact name of a biome in the target Planet Data asset.

| Application | Behavior |
| --- | --- |
| `Material + Foliage` | Changes final material and foliage biome weights without evaluating the target biome's height graph. |
| `Generation Biome` | Replaces qualifying cells in the planet-specific composed Voronoi biome map before normal terrain generation. |

For Material + Foliage, `Biome Mode` can additively blend the selected biome or replace the existing weights. `Biome Strength`, radial falloff, texture mask, and brush opacity control the influence.

Generation Biome works on discrete cells. `Biome Cell Mask Threshold` controls which cells are replaced, so boundary detail depends on the `Biome Cell Resolution` configured on `Planet Biome Mask Output`.

## Vertex Colors

R, G, B, and A can be enabled independently on the same brush. Each channel has an override value from `0` to `1` and an independent invert option.

The brush blends the original generated channel toward the configured value. A zero texture-mask value leaves the original vertex color unchanged.

## Overlap and LOD Behavior

Overlapping brushes are evaluated deterministically by `Priority` and stable brush identity. Higher-priority brushes are evaluated later, which matters for Replace, Flatten, Multiply, Cutout, biome, and vertex-color operations.

`Recursion Levels From Max` controls which terrain LODs evaluate the brush:

- `0`: every LOD
- `1`: only `Max Recursion Level`
- higher values: that many finest LOD levels; for example, `2` affects the finest level and the next coarser level

Restricting a brush to fine LODs can cause a visible transition when terrain changes LOD.

## Cache and Editing

The Planet Spawner compiles brushes into persistent planet-owned data. Moving a brush, editing its properties, deleting it, duplicating it, and undo or redo update the affected region automatically.

`Rebuild Height Brush Cache` recompiles every brush targeting the selected planet. Use it for recovery or source changes that occurred outside normal editor notifications.

For editor scripting:

- call `Begin Height Brush Bulk Edit` before changing many brushes
- call `End Height Brush Bulk Edit` after the last change
- call `Commit Brush Changes` after Blueprint-authored changes that do not pass through normal construction or property notifications

These functions are editor-only. Runtime and cooked builds consume the compiled cache and do not contain the source brush actors.

## Performance

The cost follows the number of brushes overlapping a local region rather than the total number distributed across the planet. Texture masks use compiled scalar buffers and do not add one texture binding per brush.
