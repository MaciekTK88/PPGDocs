# Runtime API

This page lists the main Blueprint/C++ entry points exposed by the plugin.

## `APlanetSpawner`

| Function | Description |
| --- | --- |
| `BuildPlanet()` | Starts planet generation. |
| `PrecomputeChunkData()` | Precomputes chunk data, including GPU textures and triangle indices. |
| `SafetyCheck()` | Validates whether the spawner can build a planet with the current setup. |
| `GetFoliageActor()` | Returns the actor that owns runtime foliage components. |
| `ClearComponents()` | Clears generated components. |
| `RegeneratePlanet()` | Clears and rebuilds the generated planet. |
| `SetChunkQuality(int32)` | Changes chunk resolution during play. Values below `1` are clamped; call `RegeneratePlanet()` to rebuild existing chunks. |
| `IsGenerationMaterialReady()` | Checks whether the generation material is ready for use. |
| `GetPlanetGenerationStatus()` | Returns phase, progress, chunk counts, elapsed time, error text, and active state. Initial collision cells are included while the spawner is configured to wait for them. |
| `UpdateVolumetricCloudParameters()` | Updates linked volumetric cloud parameters. |
| `SetOceanSimulationEnabled(bool)` | Enables or disables the native GPU ocean simulation. |
| `SetNiagaraWaveSimulationEnabled(bool)` | Deprecated compatibility wrapper for `SetOceanSimulationEnabled(bool)`. |
| `SetNewFloatingWorldOrigin(FVector)` | Makes a current local-world position the new local zero through PPG's double-precision floating origin. Returns whether the shift succeeded. |
| `SetNewWorldOrigin(FVector)` | Compatibility wrapper around `SetNewFloatingWorldOrigin`. |

Editor-only brush functions:

| Function | Description |
| --- | --- |
| `RebuildHeightBrushCache()` | Recompiles all `Planet Height Brush` actors targeting this planet. Normal property and transform edits synchronize automatically. |
| `BeginHeightBrushBulkEdit()` | Defers repeated brush-cache work while an editor script changes many brushes. |
| `EndHeightBrushBulkEdit()` | Ends a bulk edit and performs the deferred cache update. Pair it with `BeginHeightBrushBulkEdit()`. |

## Events

| Event | Description |
| --- | --- |
| `OnPlanetGenerationFinished` | Broadcast after initial visible terrain and foliage generation finishes. When `Wait For Initial Collision Before Generation Finished` is enabled, it also waits for the collision cells requested on the first runtime generation tick. |

## `UPPGPlanetCollisionInvokerComponent`

Add this component to actors that require nearby terrain or foliage collision. It registers with an explicit `Target Planet` or periodically resolves the nearest planet surface.

| Function | Description |
| --- | --- |
| `GetResolvedPlanet()` | Returns the planet currently receiving this component's collision requests. |
| `Activate()` | Enables the component and refreshes its planet binding. |
| `Deactivate()` | Stops requests and unregisters the component from its resolved planet. |

The component exposes `Collision Radius`, `Height Margin`, `Height Deactivation Hysteresis`, `Height Prediction Time`, and `Automatic Planet Search Interval` as Blueprint-readable and writable properties.

## `APlanetHeightBrush`

`Planet Height Brush` actors are editor-only sources. Their compiled data is stored on the target Planet Spawner for runtime and cooked builds.

| Function | Description |
| --- | --- |
| `CommitBrushChanges()` | Applies Blueprint-authored editor property changes to the target planet cache. Construction, Details-panel edits, transforms, undo, and redo normally synchronize automatically. |

## `UPlanetData`

Editor callable functions:

| Function | Description |
| --- | --- |
| `RebuildPlanetPipeline()` | Synchronizes material output pins, compiles materials, rebuilds biome map, saves assets, and regenerates placed planets. |
| `RefreshBiomeCellMap()` | Rebuilds only the biome-cell texture, then saves and regenerates placed planets. |

## `UChunkObject`

`UChunkObject` is a runtime object for individual chunks. Most projects should interact with the spawner rather than calling chunk functions directly.

Blueprint-callable functions include:

- `GenerateChunk()`
- `CompleteChunkGeneration()`
- `AddWaterChunk()`
- `AssignComponents()`
- `BeginSelfDestruct()`
- `FreeComponents()`
- `SelfDestruct()`

## `AGravityController`

| Function | Description |
| --- | --- |
| `GetGravityRelativeRotation(FRotator, FVector)` | Converts world-space rotation to gravity-relative rotation. |
| `GetGravityWorldRotation(FRotator, FVector)` | Converts gravity-relative rotation to world-space rotation. |

## `UPPGFloatingOriginSubsystem`

Access this world subsystem when floating-origin control or coordinate conversion is needed independently of a planet spawner.

| Function or Event | Description |
| --- | --- |
| `ShiftWorldOrigin(FVector)` | Makes the supplied current local position the new local zero. |
| `GetGlobalOrigin()` | Returns the absolute position currently represented by local zero. |
| `LocalToGlobal(FVector)` | Converts a local position to PPG global coordinates. |
| `GlobalToLocal(FVector)` | Converts a PPG global position to local coordinates. |
| `OnPreFloatingOriginShift` | Broadcast before a shift with `WorldOffset` and `NewGlobalOrigin`. |
| `OnPostFloatingOriginShift` | Broadcast after a shift with the same values. |
