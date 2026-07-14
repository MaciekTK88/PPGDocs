# Procedural Planet Generation

Procedural Planet Generation (PPG) is an Unreal Engine plugin for building large spherical worlds from runtime-generated terrain chunks, material-driven elevation, biome masks, foliage, water, and optional volumetric cloud integration.

The plugin contains runtime code, shaders, editor-facing data assets, material nodes, and example content. Its main workflow is centered around a `Planet Data Asset` assigned to a `Planet Spawner` actor.

![Example planet rendered with PPG](assets/images/hero-example-planet.jpg)

## What PPG Provides

- Runtime spherical terrain generation with streamed chunk meshes, configurable LOD recursion, collision support, and editor-time regeneration.
- Material-driven planet authoring with custom nodes for elevation, biome masks, vertex colors, biome material blending, planet-space coordinates, water, and underwater effects.
- Planet-wide biome generation with up to 16 biome layers, Voronoi cell controls, smooth transitions, height blending, and name-synced material outputs.
- Biome-based foliage spawning with mesh variants, density controls, slope and height filters, vertex-color masks, cross-chunk clustering, per-entry LOD/WPO settings, and CPU or GPU rendering paths.
- Planetary ocean support with generated water meshes, water skirts, custom-depth underwater coverage, water material nodes, and Niagara wave simulation parameters.
- Rendering options for standard static mesh chunks, Nanite terrain chunks, collision, and ray tracing proxy generation for Hardware Lumen projects.
- Volumetric cloud parameter helpers, gravity-relative controller utilities, and a double-precision floating origin for spherical-world gameplay.
- Blueprint-readable generation phase, progress, timing, and error reporting for loading screens and runtime diagnostics.
- Example content, materials, data assets, and an example level to use as a working reference.

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
