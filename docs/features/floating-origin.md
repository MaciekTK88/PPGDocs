# Floating Origin

PPG includes a double-precision floating-origin system for keeping the player and rendered planet near local `(0, 0, 0)` without relying on Unreal's integer `UWorld::OriginLocation`.

## Shifting From a Planet Spawner

Call `Set New Floating World Origin` on `Planet Spawner` with a position in the current local world. Passing the player's current location makes that location the new local zero. The function returns false if the shift is temporarily unsafe.

`Set New World Origin` remains as a compatibility wrapper. New integrations should use the explicitly named floating-origin function.

## Using the World Subsystem

Get `PPG Floating Origin Subsystem` from the world to use:

| API | Purpose |
| --- | --- |
| `Shift World Origin` | Shift loaded world state and accumulate the global origin. |
| `Get Global Origin` | Absolute coordinate represented by local zero. |
| `Local To Global` | Convert a current local coordinate to persistent PPG global space. |
| `Global To Local` | Convert a persistent PPG global coordinate for local spawning/rendering. |
| `On Pre Floating Origin Shift` | Update systems that need notification before the shift. |
| `On Post Floating Origin Shift` | Update systems after the shift is complete. |

Both events provide the applied `World Offset` and the `New Global Origin`.

## What Is Shifted

The subsystem updates the render scene, supported physics scene, currently loaded levels, navigation, world-owned components without actors, line batchers, and physics fields. The planet spawner also refreshes planet/foliage render data and cloud parameters after its wrapper completes.

## Integration Limits

!!! warning "This is separate from Unreal's native origin rebasing"
    PPG deliberately leaves `UWorld::OriginLocation` unchanged. Native origin-aware replication helpers and unloaded World Partition cells therefore do not know about its accumulated origin.