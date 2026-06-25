# Procedural Planet Generation

Procedural Planet Generation (PPG) is an Unreal Engine plugin for building large spherical worlds from runtime-generated terrain chunks, material-driven elevation, biome masks, foliage, water, and optional volumetric cloud integration.

The plugin contains runtime code, shaders, editor-facing data assets, material nodes, and example content. Its main workflow is centered around a `Planet Data Asset` assigned to a `Planet Spawner` actor.

![Example planet rendered with PPG](assets/images/hero-example-planet.jpg)

## What PPG Provides

- A `Planet Data Asset` for planet dimensions, LOD, materials, biome layers, water, and generated biome data.
- A `Planet Spawner` actor that generates, updates, and destroys runtime terrain chunks.
- Custom `PPG` material nodes for planet-space position, terrain elevation, biome masks, biome material blending, vertex colors, water shading, and underwater post processing.
- Chunked terrain generation with static mesh or Nanite chunk output, collision generation, Hardware Lumen ray tracing proxy support, and object pools.
- Biome-driven foliage using foliage data assets, mesh variants, placement filters, vertex-color density masks, LOD entries, and optional GPU foliage components.
- Optional Niagara-driven ocean wave simulation and planetary water materials.
- Example content under the plugin's `Content/Example` folder.

## Main Workflow

1. Place the planet spawner Blueprint in a level.
2. Create or duplicate a `Planet Data Asset`.
3. Assign the required planet, generation, and biome mask materials.
4. Configure biome layers and matching material output entry names.
5. Run `Rebuild Planet Pipeline`.
6. Assign the Planet Data asset to the placed spawner.
7. Build or regenerate the planet.

## Repository Layout

```text
ProceduralPlanetGeneration/
  Config/
  Content/
    Example/
    Materials/
    Water/
    Clouds/
  Shaders/
  Source/
    PPG/
    ComputeShader/
    VoxelCore/
```

`PPG` is the main runtime module. `ComputeShader` contains the planet compute shader dispatch/readback layer. `VoxelCore` is bundled support code derived from VoxelCore, so users do not need to install a separate VoxelCore plugin.
