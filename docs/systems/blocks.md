# Block System Specification

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.protocol.BlockType`.
> **SOURCE**: Decompiled from `HytaleServer.jar`.

## 1. Class Definition (`BlockType`)

Target Class: `com.hypixel.hytale.protocol.BlockType`
Size: ~102KB (Fixed Block: 163 bytes)

```java
public class BlockType {
    // --- Identification ---
    @Nullable public String name;              // Internal Name
    public int group;                          // Group ID
    public boolean unknown;

    // --- Visuals ---
    @Nonnull public DrawType drawType;         // e.g., SOLID, TRANSPARENT
    @Nonnull public Opacity opacity;           // VISIBLE, INVISIBLE
    @Nullable public String model;             // .blockymodel path
    @Nullable public ModelTexture[] modelTexture;
    public float modelScale;
    @Nullable public ColorLight light;         // Light emission
    @Nullable public Tint tint;                // Block tint
    @Nullable public BlockTextures[] cubeTextures; // Six-sided textures

    // --- Physics & Gameplay ---
    @Nonnull public BlockMaterial material;    // ROCK, WOOD, etc.
    public int hitbox;                         // Hitbox ID/Type
    public int interactionHitbox;              // Interaction Hitbox
    @Nullable public BlockMovementSettings movementSettings; // Friction, etc.
    @Nullable public BlockFlags flags;         // Boolean flags
    
    // --- Systems ---
    @Nullable public Map<InteractionType, Integer> interactions;
    @Nullable public Map<String, Integer> states; // Block States (Key -> ID)
    @Nullable public BlockPlacementSettings placementSettings;
    
    // ... (See Decompiled Source)
}
```

## 2. Block Properties / States

Block states are mapped in `states`.
Common Properties:
- `facing`: `north`, `south`, `east`, `west`, `up`, `down`
- `lit`: `true`, `false`
- `half`: `upper`, `lower`

## 3. Sub-Objects

### `BlockMaterial` (Enum)
Values: `AIR`, `STONE`, `EARTH`, `WOOD`, `PLANT`, `WATER`, `LAVA`, `IRON`, ...

### `DrawType` (Enum)
Values: `Empty`, `Solid`, `Transparent`, `Translucent`

### `BlockPlacementSettings`
Fields: `rotationMode` (BlockPlacementRotationMode)

## 4. Asset Schema (JSON)

File Path: `Server/Item/Block/Blocks/[block_name].json`

```json
{
  "name": "example:custom_block",
  "material": "STONE",
  "drawType": "SOLID",
  "textures": {
    "all": "example:textures/block/custom_block"
  },
  "placement": {
    "rotation": "faced"
  }
}
```
