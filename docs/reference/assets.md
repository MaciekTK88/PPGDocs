# Asset Reference

## Main Authoring Assets

| Asset Type | Class | Purpose |
| --- | --- | --- |
| Planet Data Asset | `UPlanetData` | Main planet setup data asset. |
| Foliage Data Asset | `UFoliageData` | Per-biome foliage configuration. |
| Water Simulation Data Asset | `UWaterSimulationData` | Niagara ocean simulation parameters. |

## Main Runtime Actor

| Actor | Class | Purpose |
| --- | --- | --- |
| Planet Spawner | `APlanetSpawner` | Generates and manages planet chunks, water, foliage, clouds, and object pools. |

The plugin includes `Content/PlanetSpawnerBP`, a ready-to-place Blueprint based on the spawner actor. In normal projects, drag this Blueprint into your level and assign a Planet Data asset.

## Included Example Assets

Common starting points:

```text
Content/Example/Level/PPGExampleLevel
Content/Example/Assets/ExamplePlanetData
Content/Example/Assets/M_PPG_ExampleGeneration
Content/Example/Assets/M_PPG_ExampleBiomeMask
Content/Example/Assets/M_PPG_ExampleSurface
Content/Example/Assets/Biomes/
```

## Water Assets

Water-related assets are grouped under:

```text
Content/Water/
```

This includes materials, render targets, material functions, Niagara modules, and effects used by the ocean simulation.

## Cloud Assets

Cloud-related assets are grouped under:

```text
Content/Clouds/
```

## Third-Party Notices

The plugin includes third-party notices under:

```text
ThirdPartyNotices/
```

VoxelCore-derived code is bundled in the plugin, and the plugin uses DeathreyCG's Niagara ocean wave simulation tutorial as the basis for the ocean simulation.
