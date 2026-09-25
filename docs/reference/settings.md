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
| `Water Simulation Data` | Native GPU ocean simulation parameter asset. Planets using the same asset share one simulation. |
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
| `Chunk Quality` | Terrain vertex resolution per chunk. Runtime changes apply when chunks are generated; use `RegeneratePlanet()` to rebuild existing chunks. |
| `Height Brush Texture Resolution Clamp` | Maximum dimension retained when compiling brush texture masks. Smaller textures keep their source resolution. |
| `Generate Collisions` | Enables terrain collision streaming around collision invokers. |
| `Wait For Initial Collision Before Generation Finished` | Delays `On Planet Generation Finished` until the collision cells requested on the first runtime generation tick are ready. |
| `Generate Editor Collisions` | Generates collision around the editor viewport camera and placed collision invokers. |
| `Editor Camera Collision Radius` | Surface radius requested around the active editor camera. Defaults to `10,000 cm`. |
| `Editor Camera Height Margin` | Additional editor-camera activation height above the resolved terrain maximum. May be negative. |
| `Match Collision Chunks To Max Recursion Level` | Uses collision-cell boundaries matching visible terrain at `Max Recursion Level`. |
| `Collision Chunk Size` | Requested collision-cell edge size in centimeters when matching is disabled. Rounded to a valid cube-face grid subdivision. |
| `Collision Chunk Quality` | Collision mesh quads per edge. `0` uses visible `Chunk Quality`. |
| `Generate Foliage Collision` | Generates physics-only foliage instances for eligible collision-enabled foliage entries. |
| `Debug Draw Collision Chunks` | Displays collision chunks as ordinary Lit meshes. |
| `Use Local Collision Height Cutoff` | Uses cached collision-cell and nearby visible-terrain height bounds for invoker altitude checks. |
| `Max Cached Collision Cell Heights` | Maximum exact collision-cell height entries retained for local altitude checks. `0` disables the persistent cache. |
| `Generate Foliage` | Generates biome foliage. |
| `Generate Ray Tracing Proxy` | Generates a ray tracing proxy for hardware ray tracing features. Required for Hardware Lumen; Software Lumen is not supported for generated planet terrain. |
| `Ray Tracing Recursion Level Margin` | Number of recursion levels below `Max Recursion Level` that also build terrain ray tracing proxy data. `0` limits proxy data to the finest chunks; higher values include additional coarser chunk levels. |
| `Async Init Body` | Uses Unreal's asynchronous physics-state pre-registration for foliage collision components when supported. |
| `Nanite Landscape` | Builds Nanite terrain chunks. Nanite can improve rendering of high-detail surfaces, but chunk build time is slower than normal static mesh chunks. |
| `Generate Second UV Channel` | Adds high-precision reconstruction UV channel. |
| `Use Full Precision UVs` | Uses 32-bit UV precision when second UV channel is disabled. |
| `Foliage Shadow Cache Mode` | `Accurate` keeps animated WPO shadows correct; `Cached` reduces invalidation at the cost of rigid shadows. |
| `Foliage Minimum Biome Blend Strength` | Ignores negligible biome contributions during foliage spawning. |
| `Global Foliage Density Scale` | Global foliage density multiplier. |
| `Enable Ocean Simulation` | Runs the native GPU ocean simulation. |
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
| `Max Collision Chunk Generation Starts Per Frame` | Maximum collision-grid jobs that can consume the shared generation-start budget each frame. |
| `Max Concurrent GPU Generations` | Hard limit for terrain/foliage GPU jobs waiting for readback. |
| `Max Concurrent Mesh Builds` | CPU mesh build concurrency limit. |
| `Foliage Upload Batch Size` | Instance transforms uploaded per chunk per frame. |
| `Max Foliage Instances Per Chunk` | Hard foliage cap per chunk. |
| `Max Pooled Chunk Objects` | Maximum idle chunk objects kept for reuse. |
| `Max Pooled Foliage ISM Components` | Maximum idle CPU foliage components kept for reuse. |
| `Max Pooled GPU Foliage Components` | Maximum idle GPU foliage components kept for reuse. |
| `Max Pooled Foliage Collision Components` | Maximum idle physics-only foliage collision components kept for reuse. |
| `Max Pooled Water Components` | Maximum idle water mesh components kept for reuse. |
| `Max Pool Objects Destroyed Per Frame` | Maximum pooled objects destroyed per frame while trimming pools. |

## Planet Height Brush

| Setting | Description |
| --- | --- |
| `Target Planet` | Planet Spawner that owns the compiled stamp. |
| `Radius` | Surface radius of the stamp in centimeters. |
| `Height Operation` | Selects Add, Subtract, Multiply, Flatten, or Cutout. |
| `Height Delta` | Displacement magnitude used by Add and Subtract. |
| `Height Multiplier` | Multiplier applied to the current procedural displacement. |
| `Flatten Height` | Target displacement from the base planet radius. |
| `Cutout Mask Threshold` | Influence at which adjoining terrain triangles, collision, and foliage are removed. |
| `Inner Radius Ratio` | Fraction of the radius receiving full influence before falloff begins. |
| `Falloff` | Linear or Smooth Step radial falloff. |
| `Priority` | Deterministic overlap order. Higher-priority brushes are evaluated later. |
| `Recursion Levels From Max` | `0` affects every LOD. Positive values select that many finest LOD levels; `1` affects only the maximum level. |
| `Enabled` | Includes or excludes the brush from the compiled planet cache. |
| `Brush Opacity` | Multiplies every enabled brush effect. |
| `Affect Height Displacement` | Enables the selected height operation or cutout. Disable it for biome and vertex-color-only use. |
| `Biome Name` | Name of the target biome in the assigned Planet Data asset. |
| `Biome Strength` | Multiplier for the biome effect. |
| `Biome Application` | Chooses Material + Foliage or Generation Biome behavior. |
| `Biome Mode` | Additively blends or replaces Material + Foliage biome weights. |
| `Biome Cell Mask Threshold` | Minimum influence required to replace a discrete Generation Biome cell. |
| `Use Texture As Biome Mask` | Multiplies biome influence by the texture mask. |
| `R / G / B / A` | Independently enables a vertex-color channel, sets its value, and optionally inverts its mask. |
| `Height Texture` | Optional scalar mask. Black preserves the original result and white applies full influence. |
| `Texture Channel` | Selects Luminance, Red, Green, Blue, or Alpha from the source texture. |
| `Texture Rotation` | Rotates the projected texture around the brush direction. |
| `Texture Scale` | Scales the projected texture footprint. |

## Planet Collision Invoker Component

| Setting | Description |
| --- | --- |
| `Target Planet` | Explicit planet to invoke. When unset, the nearest planet surface is selected automatically. |
| `Collision Radius` | Radius of the collision request projected onto the planet surface. Defaults to `10,000 cm`. |
| `Height Margin` | Additional collision activation height above the resolved terrain maximum. Defaults to `25,000 cm` and may be negative. |
| `Height Deactivation Hysteresis` | Additional retained activation height that prevents rapid collision churn. |
| `Height Prediction Time` | Extends activation using inward radial velocity. |
| `Automatic Planet Search Interval` | Delay between nearest-planet searches when `Target Planet` is unset. |

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
| `Height Cutoff Smoothness` | Non-negative feather width in centimeters inside both height cutoffs. `0` uses an inclusive hard cutoff. |
| `Min Slope` / `Max Slope` | Slope placement range. |
| `Slope Cutoff Smoothness` | Non-negative feather width on the existing slope scale inside both slope cutoffs. `0` uses an inclusive hard cutoff. |
| `Enable Collision` | Enables collision for this foliage. |
| `Collision Range` | Maximum surface distance from a collision invoker in centimeters. A negative serialized value derives its range from the deprecated recursion-distance setting. |
| `Force CPU ISMC` | Forces CPU-backed instancing. |
| `Force Mesh LOD 0` | Enabled by default. When enabled, all instances in each CPU ISMC or GPU-indirect component are forced to internal mesh LOD 0. When disabled, Unreal normally selects LOD per instance, allowing different LODs in the same component simultaneously; without GPU per-instance LOD support, selection falls back to per component. Independent of the PPG `LODs` mesh array. |
| `Enable WPO` / `WPO Disable Distance` | Enables material WPO and optionally disables it beyond a hard distance. |
| `Visible In Ray Tracing` | Includes the foliage in ray tracing effects. |
| `Pass Terrain Vertex Color To Custom Data` | Writes terrain vertex color to per-instance custom data. |
| `LODs` | Alternate aligned mesh arrays with activation distance, WPO state, and density scale. |
| `Enable Clustering` | Enables deterministic cross-chunk foliage clusters. |
| `Cluster Size Min` / `Max` | Number of members per cluster. |
| `Cluster Radius` | Spatial spread of each cluster. |
