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
| `UpdateVolumetricCloudParameters()` | Updates linked volumetric cloud parameters. |
| `SetNiagaraWaveSimulationEnabled(bool)` | Enables or disables the Niagara water simulation. |
| `SetNewWorldOrigin(FVector)` | Updates origin handling for large-world behavior. |

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

Notable functions include:

- `GenerateChunk()`
- `CompleteChunkGeneration()`
- `UploadChunk()`
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

