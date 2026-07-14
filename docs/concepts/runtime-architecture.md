# Runtime Architecture

The plugin generates a planet as a tree of runtime chunks. Each chunk represents a patch of the sphere at a specific recursion level.

## Main Runtime Objects

| Object | Role |
| --- | --- |
| `APlanetSpawner` | Level actor that owns generation settings, chunk trees, water simulation, pools, and high-level build/regenerate functions. |
| `UPlanetData` | Data asset containing planet dimensions, LOD range, materials, biome layers, water settings, and generated biome-cell map. |
| `UChunkObject` | Runtime object representing one terrain chunk and its generated mesh, water mesh, foliage, collision, and generation state. |
| `UFoliageData` | Data asset describing foliage meshes, density, placement limits, rendering options, and LOD entries. |
| `UWaterSimulationData` | Data asset containing parameters uploaded to the Niagara ocean simulation system. |
| `UPPGFloatingOriginSubsystem` | World subsystem that tracks a double-precision global origin and shifts loaded world state. |

## Chunk Generation Flow

1. `Planet Spawner` starts or ticks generation.
2. The chunk tree decides which cube-face chunks should exist for the current view and recursion settings.
3. Each `UChunkObject` initializes generation settings such as chunk location, rotation, size, terrain quality, water settings, foliage limits, and collision settings.
4. The terrain compute shader evaluates the generation material over the chunk vertex grid.
5. GPU output is read back into CPU arrays for vertex positions, vertex colors, packed normals, biome indices, and slopes. UVs are generated on the CPU.
6. The chunk builds a static mesh or Nanite mesh from the generated positions, packed normals, UVs, vertex colors, and shared triangle indices.
7. Collision and ray tracing data are created when enabled.
8. The chunk assigns the terrain component, creates/updates the dynamic terrain material instance, and sets runtime parameters such as `BiomeMap`, `PlanetRadius`, `NoiseHeight`, and chunk transform data.
9. Water mesh components are added when water is enabled and the chunk intersects the water range.
10. Foliage generation/upload runs when foliage is enabled.
11. Finished components are registered in the level and unused pooled objects are trimmed over time.

## Object Pools

The spawner keeps pools for chunk objects, foliage components, GPU foliage components, and water mesh components. These pools improve performance by reusing objects instead of constantly creating and destroying them as chunks appear, disappear, and change LOD.

Key pool settings:

- `Max Pooled Chunk Objects`
- `Max Pooled Foliage ISM Components`
- `Max Pooled GPU Foliage Components`
- `Max Pooled Water Components`
- `Max Pool Objects Destroyed Per Frame`

## Performance Gates

The spawner also limits work started or completed per frame:

- `Max Chunk Completions Per Frame`
- `Max Chunk Generation Starts Per Frame`
- `Max Concurrent GPU Generations`
- `Max Concurrent Mesh Builds`
- `Foliage Upload Batch Size`
- `Max Foliage Instances Per Chunk`

These settings are important when tuning large planets or dense foliage because they control spikes in CPU work, GPU readbacks, mesh builds, and component uploads.

## Floating-Origin Flow

PPG's floating-origin subsystem does not modify Unreal's integer `UWorld::OriginLocation`. It shifts the scene, physics, every currently loaded level, navigation, world-owned components, line batchers, and physics fields, then accumulates the shift in a double-precision `Global Origin`.

Use `Local To Global` and `Global To Local` for persistent coordinates. Native origin-aware replication and unloaded World Partition cells do not know about the PPG origin and require project-specific integration.
