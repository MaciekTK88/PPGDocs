# Runtime Architecture

The plugin generates a planet as a tree of runtime chunks. Each chunk represents a patch of the sphere at a specific recursion level.

## Main Runtime Objects

| Object | Role |
| --- | --- |
| `APlanetSpawner` | Level actor that owns generation settings, chunk trees, collision grid, compiled brush cache, pools, and high-level build/regenerate functions, and acquires a shared water simulation when needed. |
| `APlanetHeightBrush` | Editor-only stamp actor compiled into persistent planet-owned runtime data. |
| `APPGWaterSimulationManager` | Automatically created per-world actor that owns and shares native ocean simulation components. It appears as `PPG Water Simulations` in the World Outliner and cannot be deleted manually. |
| `UPPGWaterSimulationComponent` | Runs one native GPU ocean simulation and owns its transient render targets. |
| `UPlanetData` | Data asset containing planet dimensions, LOD range, materials, biome layers, water settings, and generated biome-cell map. |
| `UChunkObject` | Runtime object representing one visible or collision-grid chunk and its generated data and state. |
| `UPPGPlanetCollisionInvokerComponent` | Actor component that requests terrain and foliage collision around its owner. |
| `UPPGFoliageCollisionComponent` | Non-rendering instanced component that owns foliage physics bodies grouped by mesh. |
| `UFoliageData` | Data asset describing foliage meshes, density, placement limits, rendering options, and LOD entries. |
| `UWaterSimulationData` | Data asset containing parameters used by the GPU ocean simulation. |
| `UPPGFloatingOriginSubsystem` | World subsystem that tracks a double-precision global origin and shifts loaded world state. |

## Chunk Generation Flow

1. `Planet Spawner` starts or ticks generation.
2. The chunk tree decides which cube-face chunks should exist for the current view and recursion settings.
3. Each `UChunkObject` initializes generation settings such as chunk location, rotation, size, terrain quality, water settings, foliage limits, and collision settings.
4. The terrain compute shader evaluates the generation material over the chunk vertex grid and applies candidate terrain stamps.
5. GPU output is read back into CPU arrays for modified vertex positions, vertex colors, packed normals, biome indices, slopes, and cutout state. UVs are generated on the CPU.
6. The chunk builds a static mesh or Nanite mesh from the generated positions, packed normals, UVs, vertex colors, and surviving triangle indices.
7. Ray tracing proxy data is created when enabled and supported.
8. The chunk assigns the terrain component, creates/updates the dynamic terrain material instance, and sets runtime parameters such as `BiomeMap`, `PlanetRadius`, `NoiseHeight`, and chunk transform data.
9. Water mesh components are added when water is enabled and the chunk intersects the water range.
10. Foliage generation/upload runs when foliage is enabled.
11. Finished components are registered in the level and unused pooled objects are trimmed over time.

## Collision Generation Flow

Collision streaming runs independently from visible quadtree traversal:

1. Active collision invokers resolve an explicit planet or periodically select the nearest planet surface.
2. The spawner projects each eligible invoker radius onto all six cube faces and resolves the intersecting canonical grid-cell keys.
3. Requests from multiple invokers are merged, so each planet cell has at most one collision chunk.
4. Invokers above the resolved local terrain maximum plus their height margin are excluded.
5. Requested cells evaluate the generation material on the GPU and read terrain and foliage attributes back to the CPU.
6. Collision mesh construction and Chaos triangle-mesh cooking run on background threads.
7. Terrain collision components and any eligible foliage collision groups are registered on the game thread.
8. Cells outside the retained request area are removed and their objects or components return to their pools.

Grid traversal is refreshed only after an invoker moves a fraction of a cell or relevant settings change. Collision jobs are scheduled before visible terrain jobs, with their own per-frame start limit, while sharing the global GPU and mesh-build concurrency limits.

## Object Pools

The spawner keeps pools for chunk objects, visible foliage components, foliage collision components, GPU foliage components, and water mesh components. These pools improve performance by reusing objects instead of constantly creating and destroying them as chunks appear, disappear, and change LOD.

Key pool settings:

- `Max Pooled Chunk Objects`
- `Max Pooled Foliage ISM Components`
- `Max Pooled GPU Foliage Components`
- `Max Pooled Foliage Collision Components`
- `Max Pooled Water Components`
- `Max Pool Objects Destroyed Per Frame`

## Performance Gates

The spawner also limits work started or completed per frame:

- `Max Chunk Completions Per Frame`
- `Max Chunk Generation Starts Per Frame`
- `Max Collision Chunk Generation Starts Per Frame`
- `Max Concurrent GPU Generations`
- `Max Concurrent Mesh Builds`
- `Foliage Upload Batch Size`
- `Max Foliage Instances Per Chunk`

These settings are important when tuning large planets or dense foliage because they control spikes in CPU work, GPU readbacks, mesh builds, and component uploads.

## Water Simulation Sharing

Each world creates one `PPG Water Simulations` manager when ocean simulation is first needed. The manager keeps one simulation component and one set of transient render targets for each distinct `Water Simulation Data` asset.

Planets that reference the same data asset acquire the same component and sample the same simulation outputs. Planets using different data assets receive independent simulations.

## Floating-Origin Flow

PPG's floating-origin subsystem does not modify Unreal's integer `UWorld::OriginLocation`. It shifts the scene, physics, every currently loaded level, navigation, world-owned components, line batchers, and physics fields, then accumulates the shift in a double-precision `Global Origin`.

Use `Local To Global` and `Global To Local` for persistent coordinates. Native origin-aware replication and unloaded World Partition cells do not know about the PPG origin and require project-specific integration.
