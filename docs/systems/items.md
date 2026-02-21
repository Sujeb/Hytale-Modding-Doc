# Item System Specification

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.protocol.ItemBase`.
> **SOURCE**: Decompiled from `HytaleServer.jar`.

## 1. Class Definition (`ItemBase`)

Target Class: `com.hypixel.hytale.protocol.ItemBase`
Size: ~100KB (Fixed Block: 147 bytes)

```java
public class ItemBase {
    // --- Identity ---
    @Nullable public String id;                // Registry Name (e.g. "my_mod:my_item")
    public int itemLevel;                      // Level requirement?
    public int qualityIndex;                   // Enum index
    @Nullable public String set;               // Item Set ID
    @Nullable public String[] categories;      // Filter categories
    @Nullable public int[] tagIndexes;         // Tag IDs

    // --- Visuals ---
    @Nullable public String model;             // Path to .blockymodel (model)
    @Nullable public String texture;           // Path to .png (texture)
    @Nullable public String icon;              // Inventory icon path
    @Nullable public String animation;         // Default animation
    public float scale;                        // Model scale factor
    @Nullable public AssetIconProperties iconProperties;
    @Nullable public ItemTranslationProperties translationProperties;
    @Nullable public ColorLight light;         // Light emission color
    @Nullable public ModelParticle[] particles;
    @Nullable public ModelParticle[] firstPersonParticles;
    @Nullable public ModelTrail[] trails;

    // --- Gameplay ---
    public int maxStack;                       // Stack size
    public double durability;                  // Max durability
    public boolean consumable;                 // Is consumable?
    public int blockId;                        // Placed block ID (if any)
    public boolean variant;                    // Has variants?

    // --- Components (Nullable) ---
    @Nullable public ItemTool tool;            // Tool properties (efficiency, tier)
    @Nullable public ItemWeapon weapon;        // Damage properties
    @Nullable public ItemArmor armor;          // Defense properties
    @Nullable public ItemGlider gliderConfig;  // Glider physics
    @Nullable public ItemUtility utility;      // Utility props
    
    // --- Interactions ---
    @Nullable public Map<InteractionType, Integer> interactions;
    @Nullable public InteractionConfiguration interactionConfig;
    
    // ... (See Decompiled Source for full list)
}
```

## 2. Asset Schema (JSON)

FileStandard path: `Server/Item/Items/my_item.json`

```json
{
  "id": "example:custom_sword",
  "model": "example:models/item/custom_sword.blockymodel",
  "texture": "example:textures/item/custom_sword.png",
  "icon": "example:textures/gui/icons/custom_sword.png",
  "rarity": "RARE",
  "maxStack": 1,
  "weapon": {
    "damage": 10.0,
    "attackSpeed": 1.6
  }
}
```

## 3. Registration Pattern

**Method**: Event-based via `HytaleServer` instance.

```java
public class ItemRegistry {
    public static void register(HytaleServer server) {
        // Option A: Direct ItemBase Construction
        ItemBase myItem = new ItemBase();
        myItem.id = "my_mod:example_item";
        myItem.maxStack = 64;
        
        // Option B: Builder/Factory (Preferred if available, else raw)
        // Note: Raw instantiation is standard in early modding.
        
        // Register event (Hypothetical - adjust to actual event system found)
        server.getEventRegistry().register(RegisterAssetsEvent.class, event -> {
             event.registerItem(myItem);
        });
    }
}
```

## 4. Key Sub-Classes

### `ItemTool`
Fields: `harvestLevel` (int), `efficiency` (float), `damage` (float)

### `ItemWeapon`
Fields: `damage` (float), `range` (float), `attackSpeed` (float)

### `ItemArmor`
Fields: `defense` (float), `slot` (EquipmentSlot)
