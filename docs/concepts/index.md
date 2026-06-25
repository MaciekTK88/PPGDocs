# Core Concepts

PPG separates planet authoring into three connected parts:

- `Planet Data Asset`: durable authoring data and generated biome map storage.
- Material pipeline: custom PPG material nodes that describe elevation, biome masks, vertex colors, surface blending, water, and underwater rendering.
- Runtime spawner: an actor that turns the data and materials into chunks, water meshes, foliage components, and optional cloud updates.

