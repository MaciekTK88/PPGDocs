# Atmospheres and Skylight

Place a `PPG Atmosphere Actor` at each planet center. Set `Ground Radius Km` to that planet's sea-level radius and adjust `Atmosphere Height Km`, ground albedo, and scattering settings. Multiple enabled actors can contribute to the sky around the viewer.

Add a `PPG Skylight Actor` for ambient light and reflections from the active sky. By default it follows the primary camera and uses planet occlusion, including when viewed from orbit. `Cubemap Resolution` controls reflection detail and capture cost. `Time Slice Capture` spreads capture work across frames but can mix capture positions during fast movement. If several PPG skylights are enabled, the one with the highest `Priority` is used.

The skylight captures one lighting environment at the viewer's position. To fade that shared light on distant receivers, enable `Limit Influence Distance` and set `Full Intensity Distance Km` and `Max Influence Distance Km`.

## Shader Hooks

Open **Project Settings > Plugins > Procedural Planet Generation - Shader Hooks** and install **Multi Atmosphere Sky and Skylight** for combined atmosphere rendering and skylight capture. **Multi Atmosphere Sunlight** adds direct sunlight correction. **Skylight Distance Influence** is required for the distance fade. Restart the editor after installing or updating hooks so the shaders recompile.

Keep **Per Pixel Atmosphere Transmittance** enabled on atmosphere directional lights. For Blueprint changes to atmosphere or skylight appearance that do not run through editor property updates, call `Refresh Atmosphere` or `Refresh Skylight`.

For volumetric clouds, see [Clouds and Gravity](clouds-and-gravity.md).
