# Planet Navigation

PPG builds local navigation meshes over generated planet terrain and navigation-relevant level geometry. Invokers request coverage around moving actors; region volumes provide authored coverage for structures or fixed areas. Navigation is separate from visible terrain LOD.

## Agents and Coverage

Define agents in **Project Settings > PPG Navigation > Agent Settings**. Give each agent a unique name, capsule radius and height, and traversal slope and step limits. A region or invoker can support all agents or selected agent names. **Generation Preset** selects mesh quality; `None` uses the project default. Restart the editor after changing agent definitions.

For moving actors:

1. Add a `PPG Navigation Invoker Component` to the actor.
2. Set its coverage and removal radii and supported agents.
3. Choose a `Generation Preset`, or leave it at `None` to use the project default generation settings.

With `Independent Navigation Generation` disabled, the navigation invoker also requests the collision cells used to build its navigation mesh. It serves as the actor's collision invoker, so a second collision invoker is unnecessary for the same area.

Enable `Independent Navigation Generation` in the selected generation settings when a large navigation area can use coarser terrain sampling than nearby physics collision. Navigation then uses its own `Cell Size` and `Navigation Patch Size` without making the navigation invoker request full collision coverage. If the actor still needs physics collision, add a separate [collision invoker](collision.md) with a radius appropriate for that need. Foliage contributes to navigation when its collision instances are present and navigation-relevant.

For a fixed area, place a `PPG Navigation Region Volume` around the structure, or add a `PPG Navigation Region Component` to an actor. Set `Target Planet` or allow automatic nearest-planet selection, then choose supported agents and `Runtime Dynamic` mode. Source meshes need collision and **Can Ever Affect Navigation** enabled. After runtime structure edits, call `Mark Navigation Dirty`; bracket a batch with `Begin Navigation Edit` and `End Navigation Edit`.

## Baked Regions

For predefined areas, create a `PPG Navigation Mesh Asset`, set the region to `Baked`, assign the asset to `Baked Navigation`, and use `Bake Navigation`. Save the asset and level or Blueprint. Rebake after changing the region shape, supported agents, or generation settings.

## AI and Blueprint Queries

Use `PPG Navigation AI Controller` for planet-aware `Move To` and Behavior Tree movement. Select its `Navigation Agent` or let it resolve one from the pawn. Wait until the relevant invoker or region is ready before requesting a route; a move submitted during generation can fail.

The **PPG > Navigation** Blueprint nodes include `Find Planet Path`, `Project Point to Planet Navigation`, `Get Random Reachable Planet Point in Radius`, and `Get Random Navigable Planet Point in Radius`. Reachable points have a route from the origin; navigable points may be on disconnected surfaces. Queries use ready coverage and do not generate meshes. Check their success or status before using a location.

For Unreal's native navigation nodes, keep a `Nav Mesh Bounds Volume` in the level so the PPG navigation data registers. PPG invokers and regions still determine the actual mesh coverage. Use the PPG controller for path following on a spherical planet.

In the editor, enable viewport realtime and press **P** to preview navigation for the **Preview Agent** selected in PPG Navigation settings. Collision-backed preview also requires `Generate Editor Collisions` on the planet.
