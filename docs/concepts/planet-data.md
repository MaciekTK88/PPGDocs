# Planet Data

`Planet Data Asset` is the central authoring asset for a planet.

Create one from the Content Browser asset menu, or duplicate the included example asset:

```text
Content/Example/Assets/ExamplePlanetData
```

## Dimensions and LOD

| Setting | Description |
| --- | --- |
| `Planet Radius` | Base radius before elevation is applied. Stored in Unreal units. |
| `Noise Height` | Converts normalized material elevation to world-space displacement. |
| `Generation Seed` | Stable seed supplied to generation material expressions. Identical inputs and seeds produce identical terrain. |
| `Min Recursion Level` | Lowest terrain recursion level. |
| `Max Recursion Level` | Highest terrain recursion level. |

## Materials

| Setting | Description |
| --- | --- |
| `Planet Material` | Visible surface material. Add `Planet Biome Material Output` when blending per-biome material attributes. |
| `Generation Material` | Computes global height, biome height outputs, and vertex colors. |
| `Biome Mask Material` | Chooses biome IDs for the baked biome-cell map. |

## Biome Layers

`Biome Layers` is an ordered list of biome entries. Each entry has:

- a display `Name`
- optional `Foliage Data`
- a stable internal identity used to preserve material connections when layers are reordered

Layer order matters. Later biome layers have priority over earlier layers in the biome mask pipeline.

Biome names also matter for material synchronization. The Planet Data biome name must match the `Entry Names` used by the biome output nodes in the biome mask, generation, and surface materials, as well as entries on `Planet Biome Strengths`. Empty names are normalized to `Biome 1`, `Biome 2`, and so on.

Named biome-strength bindings are stored per Planet Data asset and applied to each terrain chunk's dynamic material instance. Planet Data assets with different biome orders can therefore use the same parent surface material.

## Settings Owned by Material Outputs

The current pipeline keeps settings next to the graph that consumes them:

| Output Node | Authored Settings |
| --- | --- |
| `Planet Biome Mask Output` | `Biome Cell Resolution` and `Biome Cell Seed`. |
| `Planet Elevation Output` | `Biome Transition`, height-based material blending, blend smoothness, and biome Voronoi warp strength/scale. |

`Planet Data` stores synchronized cooked caches of these values. Edit the output nodes, then run `Rebuild Planet Pipeline`; do not treat the hidden cache fields as authoring controls.

## Generated Data

The asset stores a generated `Biome Cell Map` texture. The baked texture is:

```text
width  = 6 * BiomeCellResolution
height = BiomeCellResolution
```

The six cube faces are packed horizontally. Each texel corresponds to one Voronoi biome cell.

## Build Buttons

| Button | Use When |
| --- | --- |
| `Rebuild Planet Pipeline` | Materials, biome layer setup, node pins, generated data, or placed planets need a full rebuild. |
| `Refresh Biome Map` | Only the generated biome-cell texture needs to be refreshed. |
