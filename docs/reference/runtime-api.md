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
| `IsGenerationMaterialReady()` | Checks whether the generation material is ready for use. |
| `GetPlanetGenerationStatus()` | Returns phase, progress, chunk counts, elapsed time, error text, and active state. |
| `UpdateVolumetricCloudParameters()` | Updates linked volumetric cloud parameters. |
| `SetNiagaraWaveSimulationEnabled(bool)` | Enables or disables the Niagara water simulation. |
| `SetNewFloatingWorldOrigin(FVector)` | Makes a current local-world position the new local zero through PPG's double-precision floating origin. Returns whether the shift succeeded. |
| `SetNewWorldOrigin(FVector)` | Compatibility wrapper around `SetNewFloatingWorldOrigin`. |

## Events

| Event | Description |
| --- | --- |
| `OnPlanetGenerationFinished` | Broadcast after initial planet generation finishes. |

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
