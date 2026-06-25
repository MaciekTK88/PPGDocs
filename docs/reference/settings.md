# Settings

This page summarizes the most important editor-facing settings.

## Planet Data Asset

| Setting | Description |
| --- | --- |
| `Planet Radius` | Base radius before elevation. |
| `Noise Height` | Normalized height to world displacement scale. |
| `Shader Precision` | Float, Double Float, or Automatic. Automatic uses float while the current planet/chunk precision is sufficient, and switches chunk generation to double-float when float precision would be too coarse for vertex spacing, height detail, or biome transition blending. |
| `Min Recursion Level` | Minimum chunk recursion level. |
| `Max Recursion Level` | Maximum chunk recursion level. |
| `Planet Material` | Visible surface material used by generated terrain chunks. |
| `Generation Material` | Computes terrain height and vertex colors. |
| `Biome Mask Material` | Chooses biome IDs for the generated biome-cell map. |
| `Biome Layers` | Ordered biome entries. Later entries have higher biome-mask priority. |
| `Biome Cell Resolution` | Voronoi cells per cube face edge. |
| `Biome Cell Seed` | Deterministic seed for biome cells. |
| `Biome Transition Smoothness` | Biome influence falloff width. |
| `Height Blend Biome Materials` | Blend material choice by strongest height contribution. |
| `Biome Material Height Blend Smoothness` | Normalized height blend smoothness. |
| `Biome Foliage Minimum Blend Strength` | Minimum biome contribution for foliage. |
| `Biome Foliage Minimum Instances Per Component` | Drop tiny foliage components. |
| `Biome Voronoi Warp Strength` | Biome lookup warp strength. |
| `Biome Voronoi Warp Scale` | Biome lookup warp frequency. |
| `Planet Position Scale` | Scale exposed through `Planet Position`. |
| `Generate Water` | Enables water settings and water generation. |
| `Water Material` | Near/primary water material. |
| `Far Water Material` | Water material used according to the material-change recursion threshold. |
| `Water Simulation Data` | Niagara ocean simulation parameter asset. |
| `Recursion Level For Material Change` | Water material switch recursion threshold. |

## Planet Spawner

| Setting | Description |
| --- | --- |
| `Use Editor Tick` | Enables editor-time generation ticking. |
| `Show Generation Debug` | Displays current generation progress and timing. |
| `Planet Data` | Planet Data asset used by the placed spawner. |
| `Chunk Quality` | Terrain vertex resolution per chunk. |
| `Generate Collisions` | Builds collision for terrain chunks. |
| `Generate Foliage` | Generates biome foliage. |
| `Generate Ray Tracing Proxy` | Generates ray tracing support where applicable. |
| `Collision Disable Distance` | Distance for disabling collision. |
| `Async Init Body` | Enables async body initialization. |
| `Nanite Landscape` | Builds Nanite landscape chunks. |
| `Generate Second UV Channel` | Adds high-precision reconstruction UV channel. |
| `Use Full Precision UVs` | Uses 32-bit UV precision when second UV channel is disabled. |
| `Global Foliage Density Scale` | Global foliage density multiplier. |
| `Enable Niagara Wave Simulation` | Runs Niagara ocean simulation. |
| `Max Recursion Water Tessellation` | High-detail water tessellation. |
| `Far Water Tessellation` | Distant water tessellation. |
| `Generate Custom Depth Water Coverage` | Adds custom-depth-only water coverage. |
| `Custom Depth Water Resolution Percent` | Custom-depth water tessellation scale. |
| `Custom Depth Water Material` | Material used by custom-depth-only water chunks. |
| `Generate Water Skirts` | Adds skirts to hide water cracks. |
| `Water Skirt Length Scale` | Water skirt length as chunk-size fraction. |
| `Volumetric Cloud Actor` | Cloud actor updated by the spawner's cloud utility. |
| `Max Chunk Completions Per Frame` | Work throttle for chunk completions. |
| `Max Chunk Generation Starts Per Frame` | Work throttle for new chunk builds. |
| `Max Concurrent Mesh Builds` | CPU mesh build concurrency limit. |
| `Foliage Upload Batch Size` | Instance transforms uploaded per chunk per frame. |
| `Max Foliage Instances Per Chunk` | Hard foliage cap per chunk. |
| `Max Pooled Chunk Objects` | Maximum idle chunk objects kept for reuse. |
| `Max Pooled Foliage ISM Components` | Maximum idle CPU foliage components kept for reuse. |
| `Max Pooled GPU Foliage Components` | Maximum idle GPU foliage components kept for reuse. |
| `Max Pooled Water Components` | Maximum idle water mesh components kept for reuse. |
| `Max Pool Objects Destroyed Per Frame` | Maximum pooled objects destroyed per frame while trimming pools. |

## Foliage Data Asset

| Setting | Description |
| --- | --- |
| `Meshes` | Mesh variants with probability weights. |
| `Foliage Density` | Spawn density for the entry. |
| `Density Vertex Color Channel` | Optional vertex-color channel density mask. |
| `Spawn Distance` | Distance at which instances are spawned. |
| `Min Scale` / `Max Scale` | Random scale range. |
| `Culling Distance` | Instance cull distance. |
| `Align To Terrain` | Aligns instances to terrain normals. |
| `Min Height` / `Max Height` | Height placement range. |
| `Min Slope` / `Max Slope` | Slope placement range. |
| `Enable Collision` | Enables collision for this foliage. |
| `Force CPU ISMC` | Forces CPU-backed instancing. |
| `Pass Terrain Vertex Color To Custom Data` | Writes terrain vertex color to per-instance custom data. |
