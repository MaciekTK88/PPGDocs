# Terrain Generation

Terrain generation is driven by the `Planet Spawner`, `Planet Data Asset`, custom material nodes, and compute shader readback.

## Chunk Quality

`Chunk Quality` controls the vertex resolution of generated terrain chunks. Higher values increase detail, but also increase:

- compute shader work
- GPU readback size
- CPU mesh build cost
- memory use
- collision build cost when collision is enabled

## Recursion Levels

The planet data asset controls:

- `Min Recursion Level`
- `Max Recursion Level`

Higher recursion levels create smaller terrain chunks and allow more local detail. They also increase the number of chunks around the viewer.

## Collision

Enable `Generate Collisions` on the spawner to build collision for generated terrain chunks.

`Collision Disable Distance` controls how far collision remains active. This helps avoid paying collision cost for distant terrain.

## Nanite

Enable `Nanite Landscape` when the project benefits from Nanite terrain rendering and the target platform supports it. Nanite chunks can render high-detail planet surfaces well, but they are slower to build than normal static mesh chunks. Expect higher generation/build cost when chunks are created or regenerated.

## Ray Tracing Proxy and Lumen

Enable `Generate Ray Tracing Proxy` when the planet needs to be visible to hardware ray tracing features. Software Lumen is not supported for the generated planet terrain.

Ray tracing proxies add build and memory cost, so keep them disabled when the project does not use Hardware Lumen or other hardware ray tracing features.

## UV Precision

The spawner exposes two terrain UV precision settings:

| Setting | Description |
| --- | --- |
| `Generate Second UV Channel` | Generates a second terrain UV channel used by `Planet UVs` for high-precision chunk UV reconstruction. Forces `Use Full Precision UVs` option to be enabled.|
| `Use Full Precision UVs` | Uses full 32-bit precision for terrain UV storage when the second UV channel is disabled. |

Use one of these if material UV precision issues appear on large planets.

![Generated terrain close-up](../assets/images/terrain-closeup.png)
