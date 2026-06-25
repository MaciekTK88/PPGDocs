# Troubleshooting

## The Planet Does Not Generate

Check:

- `Planet Spawner` has a valid `Planet Data` asset assigned.
- `Planet Data` has valid `Planet Material`, `Generation Material`, and `Biome Mask Material` assignments.
- The generation material has the required output nodes.
- The biome mask material has exactly one `Planet Biome Mask Output`.
- `Safety Check` succeeds on the spawner.
- Materials are compiled before generation starts.

## Biome Pins Do Not Match the Biome List

Run:

```text
Rebuild Planet Pipeline
```

This updates biome pins on the custom output nodes and preserves connections through stable biome identities where possible.

## Biome Changes Are Not Visible

Use:

```text
Refresh Biome Map
```

if the biome mask material or biome-cell settings changed. Use `Rebuild Planet Pipeline` for broader material or biome layer changes.

In the editor, applying or recompiling linked materials should usually update placed planets automatically. Biome mask material recompiles refresh the biome map and regenerate; generation material recompiles regenerate; surface material recompiles refresh the surface biome entry map when the material contains `Planet Biome Material Output`.

## Foliage Is Missing

Check:

- `Generate Foliage` is enabled on the spawner.
- The relevant biome has a `Foliage Data Asset`.
- Foliage density is greater than zero.
- Height and slope filters are not excluding the terrain.
- `Biome Foliage Minimum Blend Strength` is not too high.
- `Max Foliage Instances Per Chunk` is not too low.
- Vertex-color density masks are producing the expected channel values.

## Water Is Missing

Check:

- `Generate Water` is enabled on the `Planet Data Asset`.
- Water materials are assigned.
- The spawner water tessellation settings are greater than zero.
- The Niagara plugin is enabled if wave simulation is expected.
- `Enable Niagara Wave Simulation` is enabled if using the simulation.

## Terrain Materials Swim or Lose Precision

For very large planets, try:

- enabling `Generate Second UV Channel`
- or enabling `Use Full Precision UVs` when the second UV channel is disabled
- using `Planet UVs` in the surface material

## Generation Causes Frame Spikes

Lower or tune:

- `Max Chunk Generation Starts Per Frame`
- `Max Chunk Completions Per Frame`
- `Max Concurrent Mesh Builds`
- `Foliage Upload Batch Size`
- `Max Foliage Instances Per Chunk`
