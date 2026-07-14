# Exposed Material Parameters

PPG creates dynamic material instances for generated terrain and water chunks, then sets a fixed set of runtime parameters. You normally do not set these by hand, but material graphs need the matching parameter names when they consume runtime planet data.

## Terrain Surface Parameters

These parameters are set on the `Planet Material` assigned in the Planet Data asset.

| Name | Type | Purpose |
| --- | --- | --- |
| `BiomeMap` | Texture | Per-chunk render target containing the three strongest biome IDs and strengths. `Planet Biome Map Sample` owns this fixed parameter; no separate parameter node is needed. |
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
| `PPG_BiomeStrengthIndex_<expression>_<entry>` | Scalar | Planet Data biome index bound to one named `Planet Biome Strengths` output. Unmatched names use `-1`. |

The `PPG_SurfaceBiomeEntryMap*` parameters are generated from matching Planet Data biome names to `Planet Biome Material Output` entry names. The output node uses them to map raw Planet Data biome IDs to its own entry order.

The `PPG_BiomeStrengthIndex_*` parameters are generated from the stable expression and entry identities on `Planet Biome Strengths`. Every terrain chunk sets them from its own Planet Data asset, so multiple planets can share one parent surface material while using different biome lists or orders.

These are implementation parameters; do not set them manually or author gameplay logic against their generated names.

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
| `BiomeMap` | Texture | `Planet Biome Map Sample` |
| `PPG_SurfaceBiomeEntryMap0..3` | Vector | `Planet Biome Material Output` |
| `PPG_BiomeStrengthIndex_<expression>_<entry>` | Scalar | `Planet Biome Strengths` |
