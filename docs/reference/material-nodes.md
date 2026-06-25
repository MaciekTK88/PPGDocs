# Material Nodes

PPG material expressions appear in the `PPG` material node category, with water nodes under `PPG|Water` and biome map sampling under `PPG|Biomes`.

## Position and UV Nodes

| Node | Purpose |
| --- | --- |
| `Planet Position` | Returns planet-space position. Can optionally use `Planet Position Scale` from the planet data asset. |
| `Planet Position LWC` | Returns large-world-coordinate planet-space position. |
| `Planet UVs` | Generates planet surface UVs. Useful for high-precision surface material mapping. |
| `Planet Warp Position` | Warps a planet-space position, usually before noise or biome lookup. |

## Noise and Height Nodes

| Node | Purpose |
| --- | --- |
| `Planet Noise` | Generates procedural planet noise with FBM, craters, and erosion-style controls. |
| `Planet Global Height` | Returns global terrain height. |
| `Planet Flatten Elevation` | Flattens elevation near water level. |
| `Planet Elevation Output` | Outputs global height plus per-biome terrain heights for generation. |

## Biome Nodes

| Node | Purpose |
| --- | --- |
| `Planet Biome Mask Output` | Outputs biome mask values used to bake the biome-cell map. |
| `Planet Biome Map Sample` | Samples the generated per-chunk `BiomeMap` texture object and returns top-three biome IDs and strengths. |
| `Planet Biome Material Output` | Blends per-biome Material Attributes from IDs and strengths produced by `Planet Biome Map Sample`. |
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
| Surface material | `Planet Biome Map Sample` connected to `Planet Biome Material Output` when using biome-specific Material Attributes. The sample node needs a texture object parameter named `BiomeMap`. |

## Biome Surface Wiring

In the surface material:

| Source | Destination |
| --- | --- |
| Texture Object Parameter `BiomeMap` | `Planet Biome Map Sample` input `BiomeMap` |
| `Planet Biome Map Sample` output `Top 3 Biome IDs` | `Planet Biome Material Output` input `Top 3 Biome IDs` |
| `Planet Biome Map Sample` output `Top 3 Strengths` | `Planet Biome Material Output` input `Top 3 Strengths` |
| Per-biome Material Attributes | Matching named biome inputs on `Planet Biome Material Output` |
