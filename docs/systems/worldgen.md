# World Generation Specification

> **CONTEXT**: Reference for `com.hypixel.hytale.server.worldgen`.
> **SOURCE**: Decompiled `HytaleWorldGenProvider` & `ChunkGeneratorJsonLoader`.

## 1. Provider Architecture

**Primary Class**: `com.hypixel.hytale.server.worldgen.HytaleWorldGenProvider`
**Loader**: `com.hypixel.hytale.server.worldgen.loader.ChunkGeneratorJsonLoader`

The system loads world generation logic from JSON configurations, allowing for data-driven terrain modification without recompiling code for basic changes.

## 2. Configuration Location

The generator looks for configuration in:
1.  `[ServerRoot]/worldgen/World.json` (Default)
2.  `[ServerRoot]/worldgen/[GeneratorName]/`

## 3. WorldGen Pipeline

The generation process is divided into phases:
1.  **Noise Generation**: Defines base heightmaps and terrain shape.
2.  **Biome Selection**: Maps (x, z) coordinates to Biome IDs.
3.  **Surface Builder**: Replaces base stone with topsoil (Grass, Sand) based on biome.
4.  **Deco/Features**: Places Trees, Ores, and Structures (Prefabs).

## 4. Customizing WorldGen

### A. JSON Configuration
(Structure driven by `ChunkGeneratorJsonLoader`)

```json
{
  "ChunkGenerator": {
    "Name": "Default",
    "Biomes": [ "hytale:plains", "hytale:forest" ],
    "Noise": { ... }
  }
}
```

### B. Custom Provider (Java)
To completely override generation, implement `IWorldGenProvider` and register it.

```java
public class MyGenProvider implements IWorldGenProvider {
    @Override
    public IWorldGen getGenerator() {
         return new MyCustomGenerator();
    }
}
```

## 5. Prefabs (Structures)
Structures are handled as "Prefabs" loaded via `WorldGenPrefabLoader`.
They are typically stored as `.blockymodel` files or internal binary assets.
