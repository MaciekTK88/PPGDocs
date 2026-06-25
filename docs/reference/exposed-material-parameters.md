# Exposed Material Parameters

PPG creates dynamic material instances for generated terrain and water chunks, then sets a fixed set of runtime parameters. You normally do not set these by hand, but material graphs need the matching parameter names when they consume runtime planet data.

## Terrain Surface Parameters

These parameters are set on the `Planet Material` assigned in the Planet Data asset.

| Name | Type | Purpose |
| --- | --- | --- |
| `BiomeMap` | Texture | Per-chunk render target containing top-three biome IDs and strengths. Use a Texture Object Parameter with this exact name for `Planet Biome Map Sample`. |
| `recursionLevel` | Scalar | Chunk recursion distance from the Planet Data maximum recursion level. |
| `PlanetRadius` | Scalar | Planet radius from the Planet Data asset. |
| `NoiseHeight` | Scalar | Height scale from the Planet Data asset. |
| `ChunkSize` | Scalar | Current chunk size in Unreal units. |
| `ComponentLocation` | Vector | High part of the chunk origin. Kept for compatibility. |
| `ComponentLocationHigh` | Vector | High part of the chunk origin used by planet-position nodes. |
| `ComponentLocationLow` | Vector | Low part of the chunk origin. |
| `ComponentLocationLWC` | Double Vector | Large-world-coordinate chunk origin used by `Planet Position LWC`. |
| `ChunkLocationHigh` | Vector | High part of the chunk's planet-space location. |
| `ChunkLocationLow` | Vector | Low part of the chunk's planet-space location. |
| `ChunkSizeHigh` | Scalar | High part of chunk size used by `Planet UVs`. |
| `ChunkSizeLow` | Scalar | Low part of chunk size used by `Planet UVs`. |
| `PlanetSpaceRotation` | Vector | Integer cube-face rotation encoded as a vector. |
| `PlanetPositionScale` | Scalar | Planet Data `Planet Position Scale`, used by `Planet Position` nodes when enabled. |
| `PPG_SurfaceBiomeEntryMap0` | Vector | Internal remap for Planet Data biome indices 0-3 to surface material output entries. |
| `PPG_SurfaceBiomeEntryMap1` | Vector | Internal remap for Planet Data biome indices 4-7 to surface material output entries. |
| `PPG_SurfaceBiomeEntryMap2` | Vector | Internal remap for Planet Data biome indices 8-11 to surface material output entries. |
| `PPG_SurfaceBiomeEntryMap3` | Vector | Internal remap for Planet Data biome indices 12-15 to surface material output entries. |

The `PPG_SurfaceBiomeEntryMap*` parameters are generated from matching Planet Data biome names to `Planet Biome Material Output` entry names. They are implementation parameters used by `Planet Biome Map Sample`; do not author gameplay logic against them.

## Water Parameters

These parameters are set on the active water material or far-water material.

| Name | Type | Purpose |
| --- | --- | --- |
| `HeightMap` | Texture | Per-chunk render target shared with terrain biome/height data for water displacement/material logic. |
| `PlanetRadius` | Scalar | Planet radius from the Planet Data asset. |
| `ComponentLocation` | Vector | High part of the chunk origin. |
| `ComponentLocationHigh` | Vector | High part of the chunk origin. |
| `ComponentLocationLow` | Vector | Low part of the chunk origin. |
| `ComponentLocationLWC` | Double Vector | Large-world-coordinate chunk origin. |
| `ChunkLocationHigh` | Vector | High part of the chunk's planet-space location. |
| `ChunkLocationLow` | Vector | Low part of the chunk's planet-space location. |
| `ChunkSizeHigh` | Scalar | High part of chunk size. |
| `ChunkSizeLow` | Scalar | Low part of chunk size. |
| `PlanetSpaceRotation` | Vector | Integer cube-face rotation encoded as a vector. |
| `PlanetPositionScale` | Scalar | Planet Data `Planet Position Scale`. |

## Cloud Parameters

When `Update Volumetric Cloud Parameters` runs, the spawner updates the cloud material:

| Name | Type | Purpose |
| --- | --- | --- |
| `PlanetLocation` | Vector | World location of the Planet Spawner actor. |
| `PlanetRadius` | Scalar | Planet radius from the Planet Data asset. |

## Parameters Created by PPG Material Nodes

Some PPG material nodes compile their own parameter references:

| Name | Type | Used By |
| --- | --- | --- |
| `PlanetPositionScale` | Scalar | `Planet Position`, `Planet Position LWC` |
| `ComponentLocationHigh` | Vector | `Planet Position` |
| `ComponentLocationLWC` | Double Vector | `Planet Position LWC` |
| `ChunkLocationHigh` / `ChunkLocationLow` | Vector | `Planet UVs` |
| `ChunkSizeHigh` / `ChunkSizeLow` | Scalar | `Planet UVs` |
| `PlanetSpaceRotation` | Vector | `Planet UVs` |
| `PPG_SurfaceBiomeEntryMap0..3` | Vector | `Planet Biome Map Sample` |

