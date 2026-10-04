# Collision Streaming

PPG generates terrain collision on a planet-wide virtual grid that is independent of the visible terrain quadtree. Every collision cell at the selected grid level has the same effective size and sampling layout. Canonical cube-face cell keys prevent multiple invokers from creating duplicate collision chunks in the same location.

Collision chunks evaluate the same generation material as visible terrain. Active collision invokers and collision-backed navigation coverage request them; chunks are removed after they leave the retained area.

## Collision Invokers

Add a `PPG Planet Collision Invoker` component to characters, vehicles, physics objects, or other actors that require nearby collision. A `PPG Navigation Invoker Component` also serves as a collision invoker when its generation settings have `Independent Navigation Generation` disabled. When that setting is enabled, add a separate collision invoker only where physics collision is needed.

| Setting | Description | Default |
| --- | --- | --- |
| `Target Planet` | Planet to request collision from. When unset, the component periodically selects the nearest planet surface. | None |
| `Collision Radius` | Radius of the requested patch projected onto the planet surface. | `10,000 cm` |
| `Height Margin` | Additional activation height above the resolved local terrain maximum. Negative values are supported. | `25,000 cm` |
| `Height Deactivation Hysteresis` | Extra height retained after activation to prevent rapid enable/disable changes. | `10,000 cm` |
| `Height Prediction Time` | Extends activation using inward radial velocity so fast-descending objects request collision earlier. | `1 s` |
| `Automatic Planet Search Interval` | Interval between nearest-planet searches when `Target Planet` is unset. | `0.5 s` |

The collision grid recalculates after an invoker moves a fraction of a cell rather than traversing all six cube faces every frame. Invokers above the local terrain maximum plus `Height Margin` do not request chunks. If local height data is unavailable, the planet-wide height range is used as a fallback.

Assign `Target Planet` when a level contains multiple nearby planets and automatic selection would be ambiguous.

## Planet Spawner Settings

| Setting | Description |
| --- | --- |
| `Generate Collisions` | Enables invoker-driven terrain collision generation. |
| `Wait For Initial Collision Before Generation Finished` | Delays `On Planet Generation Finished` until the collision cells requested on the first runtime generation tick are ready. |
| `Match Collision Chunks To Max Recursion Level` | Uses the same cell boundaries as visible terrain at `Max Recursion Level`. |
| `Collision Chunk Size` | Requested cell edge size in centimeters when matching is disabled. The value is rounded to the nearest valid power-of-two cube-face subdivision. |
| `Collision Chunk Quality` | Collision mesh quads per edge. `0` uses visible `Chunk Quality`; matching to maximum recursion caps the requested value at visible chunk quality. |
| `Generate Foliage Collision` | Generates physics bodies for collision-enabled foliage entries inside collision chunks. |
| `Use Local Collision Height Cutoff` | Uses cached collision-cell and nearby visible-terrain height bounds for altitude activation. |
| `Max Cached Collision Cell Heights` | Limits retained exact cell-height entries. `0` disables the persistent cache. |
| `Async Init Body` | Uses Unreal's asynchronous physics-state pre-registration for foliage collision components where supported. |
| `Max Collision Chunk Generation Starts Per Frame` | Limits collision jobs started per frame. Collision shares the global GPU and mesh-build limits. |
| `Debug Draw Collision Chunks` | Renders active collision chunks as ordinary Lit meshes. |

Matching collision cells to maximum-recursion terrain boundaries is the safest default for consistent sampling alignment. A custom cell size can reduce the number of chunks or tighten streaming coverage, but it is still quantized to the planet's cube-face grid.

## Runtime Generation and Loading

Terrain sampling is dispatched to the GPU and read back asynchronously. Collision mesh construction and Chaos triangle-mesh cooking run on background threads. Final Unreal object setup and component registration run on the game thread.

When `Wait For Initial Collision Before Generation Finished` is enabled, the spawner snapshots the collision cells requested on the first runtime generation tick and retains them until they reach the final `READY` state. This state includes foliage collision registration when foliage collision is enabled. An empty initial request does not delay the event.

`Get Planet Generation Status` includes these initial collision cells in completed and total chunk counts. Collision continues to stream around invokers after initial planet loading finishes.

## Editor Collision

`Generate Editor Collisions` generates collision around the active editor viewport camera and placed collision invokers. It is enabled by default. `Editor Camera Collision Radius` and `Editor Camera Height Margin` control the camera request.

Hidden collision chunks participate in editor placement traces, so dragged actors and surface snapping can use the generated terrain without rendering the chunks. Enable `Debug Draw Collision Chunks` when the collision mesh needs to be inspected directly.

## Foliage Collision

Foliage collision requires all of the following:

- `Generate Collisions` on the Planet Spawner
- `Generate Foliage Collision` on the Planet Spawner
- `Enable Collision` on the foliage entry
- a positive resolved `Collision Range` on the foliage entry

Collision foliage uses the base mesh variants rather than distance-based PPG foliage LOD substitutions. It evaluates the same height, slope, vertex-color density mask, alignment, scale, depth-offset, and clustering rules as rendered foliage without distance-LOD thinning. Physics instances are grouped by static mesh into non-rendering instanced components.

`Collision Range` is measured along the planet surface from an invoker. Range relevance is evaluated per collision cell, so an intersecting cell can contain collision foliage across its complete area. Smaller collision cells provide tighter range boundaries at the cost of more chunks.

When `Collision Range` remains negative on a serialized asset, PPG derives a centimeter range from its deprecated recursion-distance value. Set an explicit centimeter value when tuning current assets.

## Diagnostics and Performance

`Show Generation Debug` reports:

- active collision chunks
- currently needed collision chunks
- pending needed chunks

Active can temporarily exceed needed because retained cells prevent rapid churn and finished removals can lag behind a changed request. Collision generation starts are limited separately, then share `Max Chunk Generation Starts Per Frame`, `Max Concurrent GPU Generations`, and `Max Concurrent Mesh Builds` with visible terrain.

For lower runtime cost:

- keep invoker radii only as large as gameplay requires
- use explicit `Target Planet` references when automatic searches are unnecessary
- keep local height cutoffs enabled
- limit foliage `Collision Range` independently for each entry
- disable foliage collision for decorative meshes
- keep collision debug rendering disabled outside diagnostics
