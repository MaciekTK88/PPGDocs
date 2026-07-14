# Material Nodes

PPG material expressions appear in the `PPG` material node category, with water nodes under `PPG|Water` and biome map sampling under `PPG|Biomes`.

## Position and UV Nodes

| Node | Purpose |
| --- | --- |
| `Planet Position` | Returns a chunk-stable float direction from the planet center for generation and biome-mask graphs. It can optionally apply `Planet Position Scale`. |
| `Planet Position LWC` | Returns per-pixel LWC planet-local position for precision-sensitive surface mapping. Use `Planet Position`, not this node, with PPG generation noise. |
| `Planet UVs` | Generates planet surface UVs. Useful for high-precision surface material mapping. |
| `Planet Warp Position` | Warps a planet-space position, usually before noise or biome lookup. |

## Noise and Height Nodes

| Node | Purpose |
| --- | --- |
| `Planet Noise` | Generates FBM, craters, Voronoi, erosion, or tectonic-plate noise. Available properties and outputs change with the selected mode. |
| `Planet Global Height` | Returns global terrain height. |
| `Planet Flatten Elevation` | Flattens elevation near water level. |
| `Planet Elevation Output` | Outputs global height plus per-biome terrain heights for generation. |

### Planet Noise Modes

| Mode | Notes |
| --- | --- |
| `FBM (Gradient + Rotation)` | General fractal terrain with octave and ridge controls. |
| `Craters` | Multi-scale crater field with lacunarity, jitter, depth, and edge rounding. |
| `Voronoi` | Cellular noise. |
| `Erosion` | Erosion-filtered terrain with 2D biome-cell or 3D triplanar mapping. It returns a packed `Height, Delta, Ridge` value. |
| `Tectonic Plates` | Plate-style large-scale structure. |

`Base Frequency` applies to every mode. `Octaves` applies to FBM, craters, and erosion. The details panel hides mode-specific properties that do not affect the selected noise type.

## Biome Nodes

| Node | Purpose |
| --- | --- |
| `Planet Biome Mask Output` | Outputs biome mask values used to bake the biome-cell map. |
| `Planet Biome Map Sample` | Owns the fixed per-chunk `BiomeMap` texture parameter and returns the three strongest biome IDs and strengths. It requires no texture input. |
| `Planet Biome Material Output` | Blends per-biome Material Attributes from IDs and strengths produced by `Planet Biome Map Sample`. |
| `Planet Biome Strengths` | Converts the sampled top-three IDs and strengths into named `0-1` biome masks. |
| `Planet Cell Origin` | Returns the current biome cell origin. |
| `Planet Cell Position` | Returns position relative to the current biome cell. |
| `Planet Biome Cell Position 2D` | Returns 2D position in the current biome cell. |

## Vertex Color and Curve Nodes

| Node | Purpose |
| --- | --- |
| `Planet Vertex Color Output` | Outputs per-biome vertex colors for terrain generation. |
| `Planet Curve Atlas Row` | Samples a curve atlas row with integer row addressing to avoid tiny precision stair steps. |

## Water Nodes

| Node | Purpose |
| --- | --- |
| `Planetary Water Shading` | Shades planetary water using light direction, water depth, scattering, absorption, normals, view direction, and horizon-aware ambient controls. |
| `Planetary Underwater Post Process` | Applies underwater post-processing using scene color, water path length, underwater mask, camera depth, light direction, and water optical coefficients. |

## Required Output Nodes

| Material | Required Node |
| --- | --- |
| Biome mask material | One `Planet Biome Mask Output`. |
| Generation material | One `Planet Elevation Output` and one `Planet Vertex Color Output`. |
| Surface material | `Planet Biome Map Sample` connected to `Planet Biome Material Output` when using biome-specific Material Attributes. |

## Biome Surface Wiring

In the surface material:

| Source | Destination |
| --- | --- |
| `Planet Biome Map Sample` output `Biome IDs` | `Planet Biome Material Output` input `Biome IDs` |
| `Planet Biome Map Sample` output `Strengths` | `Planet Biome Material Output` input `Biome Strengths` |
| Per-biome Material Attributes | Matching named biome inputs on `Planet Biome Material Output` |

To use individual biome masks, connect the same `Biome IDs` and `Strengths` outputs to `Planet Biome Strengths` inputs `Planet Biome IDs` and `Biome Strengths`. Set its `Biome Count` and entry names to the Planet Data biome names, then use its named scalar outputs elsewhere in the surface graph. Missing and non-contributing biomes output zero.

`Planet Biome Mask Output` owns biome cell resolution/seed. `Planet Elevation Output` owns transition, height blend, and biome-warp controls. Rebuild the planet pipeline after changing them.

Use one `Planet Biome Map Sample` for all consumers in a surface material. Separate sample nodes repeat the per-pixel texture sampling and biome sorting work.
