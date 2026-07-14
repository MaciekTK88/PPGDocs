# Settings

This page summarizes the most important editor-facing settings.

## Planet Data Asset

| Setting | Description |
| --- | --- |
| `Planet Radius` | Base radius before elevation. |
| `Noise Height` | Normalized height to world displacement scale. |
| `Generation Seed` | Stable seed used by terrain material expressions. |
| `Min Recursion Level` | Minimum chunk recursion level. |
| `Max Recursion Level` | Maximum chunk recursion level. |
| `Planet Material` | Visible surface material used by generated terrain chunks. |
| `Generation Material` | Computes terrain height and vertex colors. |
| `Biome Mask Material` | Chooses biome IDs for the generated biome-cell map. |
| `Biome Layers` | Ordered biome entries. Later entries have higher biome-mask priority. |
| `Planet Position Scale` | Scale exposed through `Planet Position`. |
| `Generate Water` | Enables water settings and water generation. |
| `Water Material` | Near/primary water material. |
| `Far Water Material` | Water material used according to the material-change recursion threshold. |
| `Water Simulation Data` | Niagara ocean simulation parameter asset. |
| `Recursion Level For Material Change` | Water material switch recursion threshold. |

## Material Output Settings

These controls moved from Planet Data to the material output nodes that use them:

| Node | Setting | Description |
| --- | --- | --- |
| `Planet Biome Mask Output` | `Biome Cell Resolution` | Voronoi cells per cube-face edge and baked map resolution. |
| `Planet Biome Mask Output` | `Biome Cell Seed` | Deterministic biome-cell layout seed. |
| `Planet Elevation Output` | `Biome Transition` | Biome influence falloff width. |
| `Planet Elevation Output` | `Height Blend Biome Materials` | Favors the material of the biome producing the highest elevation. |
| `Planet Elevation Output` | `Biome Material Height Blend Smoothness` | Normalized height range over which lower materials fade. |
| `Planet Elevation Output` | `Biome Voronoi Warp Strength` | Biome lookup warp strength; zero disables it. |
| `Planet Elevation Output` | `Biome Voronoi Warp Scale` | Biome lookup warp frequency. |

## Planet Spawner

| Setting | Description |
| --- | --- |
| `Use Editor Tick` | Enables editor-time generation ticking. |
| `Show Generation Debug` | Displays current generation progress and timing. |
| `Planet Data` | Planet Data asset used by the placed spawner. |
| `Chunk Quality` | Terrain vertex resolution per chunk. |
| `Generate Collisions` | Builds collision for terrain chunks. |
| `Generate Foliage` | Generates biome foliage. |
| `Generate Ray Tracing Proxy` | Generates a ray tracing proxy for hardware ray tracing features. Required for Hardware Lumen; Software Lumen is not supported for generated planet terrain. |
| `Ray Tracing Recursion Level Margin` | Number of recursion levels below `Max Recursion Level` that also build terrain ray tracing proxy data. `0` limits proxy data to the finest chunks; higher values include additional coarser chunk levels. |
| `Collision Disable Distance` | Distance for disabling collision. |
| `Async Init Body` | Enables async body initialization. |
| `Nanite Landscape` | Builds Nanite terrain chunks. Nanite can improve rendering of high-detail surfaces, but chunk build time is slower than normal static mesh chunks. |
| `Generate Second UV Channel` | Adds high-precision reconstruction UV channel. |
| `Use Full Precision UVs` | Uses 32-bit UV precision when second UV channel is disabled. |
| `Foliage Shadow Cache Mode` | `Accurate` keeps animated WPO shadows correct; `Cached` reduces invalidation at the cost of rigid shadows. |
| `Foliage Minimum Biome Blend Strength` | Ignores negligible biome contributions during foliage spawning. |
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
| `Max Concurrent GPU Generations` | Hard limit for terrain/foliage GPU jobs waiting for readback. |
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
| `Uniform Scale` / `Scale` | Chooses uniform or per-axis random scaling inside the interval. |
| `Culling Distance` | Instance cull distance. |
| `Align To Terrain` | Aligns instances to terrain normals. |
| `Min Height` / `Max Height` | Height placement range. |
| `Min Slope` / `Max Slope` | Slope placement range. |
| `Enable Collision` | Enables collision for this foliage. |
| `Force CPU ISMC` | Forces CPU-backed instancing. |
| `Enable WPO` / `WPO Disable Distance` | Enables material WPO and optionally disables it beyond a hard distance. |
| `Visible In Ray Tracing` | Includes the foliage in ray tracing effects. |
| `Pass Terrain Vertex Color To Custom Data` | Writes terrain vertex color to per-instance custom data. |
| `LODs` | Alternate aligned mesh arrays with activation distance, WPO state, and density scale. |
| `Enable Clustering` | Enables deterministic cross-chunk foliage clusters. |
| `Cluster Size Min` / `Max` | Number of members per cluster. |
| `Cluster Radius` | Spatial spread of each cluster. |
