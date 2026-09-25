# Planet Camera

Planet Camera provides planet-relative navigation in the Level Editor. It keeps the viewport horizon aligned to the surface, allowing the camera to rotate and move naturally at the poles, equator, and anywhere between them.

The mode is editor-only and does not affect runtime camera behavior or packaged builds.

## Enable Planet Camera

1. Activate a perspective level viewport.
2. Select a valid Planet Spawner. This step is optional when only one planet is nearby.
3. Click **Planet Camera** in the Level Editor toolbar.

The selected Planet Spawner is used when possible. Otherwise, the nearest valid planet in the viewport world is targeted. The target remains locked until it is retargeted or becomes invalid.

The same commands are available under **Tools > Procedural Planet Generation**.

## Controls

Hold the right mouse button to use flight navigation.

| Input | Action |
| --- | --- |
| Mouse | Yaw and pitch relative to the planet surface. |
| `E` / `Q` | Move radially away from or toward the planet. |

Camera speed, acceleration, mouse sensitivity, inverted axes, and distance-based camera speed follow the Level Editor viewport settings.

## Retarget the Camera

Select another Planet Spawner and click **Retarget Planet Camera**. If no valid planet is selected, the nearest valid planet is used.

Retargeting is explicit, so flying near another planet does not switch the camera target unexpectedly. If the target is removed, the camera attempts to use another valid planet in the same editor world.

## Actor Placement

Actors placed while Planet Camera is enabled are rotated so their local up axis points away from the center of the targeted planet. Their forward direction is preserved along the surface where possible.

Planet Spawner actors are excluded from this automatic alignment.

## Returning to Normal Navigation

Click **Planet Camera** again to disable the mode. The viewport is leveled against world Z before normal editor navigation resumes.

The mode is stored per perspective viewport for the current editor session and remains enabled when saving or temporarily switching to another editor window.

## Unaffected Editor Tools

Planet Camera changes only unmodified right-mouse flight navigation and radial `E`/`Q` movement. It does not replace:

- selection and transform gizmos
- middle-mouse panning
- Alt orbit controls
- orthographic viewports
- actor or cinematic camera piloting
- PIE or SIE navigation
- VR and asset-editor viewports
