# Foliage

Foliage is configured per biome through `Foliage Data Asset` references.

## Foliage Data Asset

A `Foliage Data Asset` contains a list of foliage entries. Each entry can define:

- one or more mesh variants
- density
- spawn distance
- scale range
- culling distance
- terrain alignment
- height and slope limits
- collision behavior
- rendering behavior
- optional LOD entries

## Mesh Variants

Each foliage entry has a `Meshes` array. Each mesh variant has:

| Field | Description |
| --- | --- |
| `Mesh` | Static mesh to spawn. |
| `Probability Weight` | Relative selection weight, normalized among valid variants in the entry. |

## Density Masks

Foliage density can be multiplied by a terrain vertex-color channel:

- `None`
- `Red`
- `Green`
- `Blue`
- `Alpha`

`Invert Density Vertex Color Mask` flips the mask so dark areas spawn more foliage and bright areas spawn less.

Use this together with `Planet Vertex Color Output` in the generation material.

## Placement Filters

Common placement controls:

| Setting | Description |
| --- | --- |
| `Min Height` / `Max Height` | Height range for spawning. |
| `Min Slope` / `Max Slope` | Slope range for spawning. |
| `Align To Terrain` | Rotates instances to terrain normals. |
| `Depth Offset` | Moves instances relative to the terrain surface. |
| `Use Absolute Height` | Treats height as always zero. |

## Runtime Limits

The spawner can globally scale or limit foliage:

- `Generate Foliage`
- `Global Foliage Density Scale`
- `Foliage Upload Batch Size`
- `Max Foliage Instances Per Chunk`
- `Max Pooled Foliage ISM Components`
- `Max Pooled GPU Foliage Components`

## GPU and CPU Rendering Paths

Foliage can use GPU foliage components when collision is disabled. Use `Force CPU ISMC` if you need CPU-backed instanced static mesh components even without collision.

`Pass Terrain Vertex Color To Custom Data` exposes nearest terrain vertex color as four per-instance custom data floats.

![Biome-specific foliage on terrain](../assets/images/foliage-biome-example.png)

![Configured Foliage Data Asset](../assets/images/foliage-data-asset.png)
