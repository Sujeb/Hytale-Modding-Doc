# Hytale Asset System

> **CONTEXT**: Documentation for the Hytale Asset Pack system.
> **SOURCE**: Analysis of `AssetModule`, `CommonAssetModule`, and `AssetRegistryLoader`.

## 1. Overview

Hytale uses a **Root-Level** asset structure, distinct from Minecraft's `assets/modid/` convention.
An Asset Pack (or Mod JAR) must have `Server/` and `Common/` directories at its root to be recognized.

*   **Server Assets** (Definitions): JSON files that define logic (Items, Blocks, Recipes). Loaded by `AssetModule`.
*   **Common Assets** (Resources): Media files (Textures, Models, Sounds) shared with the client. Loaded by `CommonAssetModule`.

## 2. Directory Structure

A valid Hytale Mod (JAR) or Resource Pack (ZIP) structure:

```
my_mod.jar (Root)
├── manifest.json            # Required: Plugin & Asset Manifest
├── Server/                  # LOGIC (JSONs)
│   ├── Item/
│   │   ├── Items/           # Item Definitions (contain BlockTypes)
│   │   │   └── my_sword.json
│   │   ├── Recipes/         # Crafting Recipes
│   │   │   └── my_recipe.json
│   │   └── Block/
│   │       └── Blocks/      # Standalone Blocks (Rare, usually in Items)
│   └── Audio/
│       └── SoundEvents/     # Sound Event Definitions (JSON)
│           └── my_sound.json
│
└── Common/                  # RESOURCES (Media)
    ├── Models/
    │   └── item/
    │       └── my_sword.blockymodel
    └── Textures/
        └── item/
            └── my_sword.png
```

## 3. Server Assets (Definitions)

These assets are strictly mapped to specific paths by `AssetRegistryLoader`.

| Asset Type | Required Path (Inside `Server/`) | Notes |
| :--- | :--- | :--- |
| **Items** | `Item/Items` | Most important. Can define Blocks inline. |
| **Blocks** | `Item/Block/Blocks` | Standalone blocks. |
| **Recipes** | `Item/Recipes` | Crafting recipes. |
| **Sound Events** | `Audio/SoundEvents` | Logical sound triggers. |
| **Particles** | `Particles` | `.particlesystem` and `.particlespawner` |
| **Models (Meta)**| `Models` | Metadata for models (not the model file). |

### A. Item Definition (Item-First Architecture)
Location: `Server/Item/Items/my_item.json`

Hytale uses an **Item-First** approach. Even blocks are primarily defined as Items with a `BlockType` property.

```json
{
  "Name": "my_mod.item.dirt",
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Cube",
    "Textures": [
      {
        "All": "Textures/blocks/dirt.png"
      }
    ]
  }
}
```

## 4. Common Assets (Resources)

The `Common/` folder is scanned recursively. Files are referenced by their path **relative to `Common/`**.

*   **File**: `Common/Textures/gui/icon.png`
*   **Reference**: `"Icon": "Textures/gui/icon.png"`

### Key Resource Types
*   **Textures**: `.png` files.
*   **Models**: `.blockymodel` or `.gltf` files.
*   **Sounds**: `.ogg` or `.wav` files.

## 5. Manifest.json

To ensure your mod's assets are loaded, your `manifest.json` (at the JAR root) must be valid.

```json
{
  "Name": "MyMod",
  "Group": "com.example.mymod",
  "ServerVersion": "1.0.0",
  "Main": "com.example.MyPlugin"
}
```
**Note**: The Hytale asset loader automatically scans for `manifest.json` at the root of JARs in the `mods/` directory.
