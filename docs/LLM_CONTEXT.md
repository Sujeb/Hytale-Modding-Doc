# LLM Context & Constraints

> **TARGET AUDIENCE**: AI Agents / LLMs. 
> **GOAL**: Provide maximum context for code generation with minimal token usage.

## 1. Modding Architecture (CRITICAL)

Hytale has **two** distinct modding layers. AI must choose the correct one based on user intent.

### A. Game Plugins (Standard)
*   **Use Case**: Adding Items, Blocks, Entities, UI, Handling Events, WorldGen, Anchor UI.
*   **Base Class**: `com.hypixel.hytale.server.core.plugin.JavaPlugin`.
*   **Configuration**: `manifest.json` in root (supports `SubPlugins` since Update 3).
*   **Loader**: `PluginManager` reads `mods/`. Supports enable/disable since Update 3.
*   **Lifecycle**: Constructor -> `setup()` (Init) -> `start()` (Post-Init) -> `shutdown()` (Cleanup). `FAILED` state on errors.

### B. Early Plugins (Advanced/Internal)
*   **Use Case**: Bytecode manipulation, Deep engine overrides, Mixins.
*   **Base Class**: `com.hypixel.hytale.plugin.early.ClassTransformer`.
*   **Recommendation**: Use **Hyxin** library (Mixins) instead of raw ASM if available.
*   **Configuration**: `META-INF/services/com.hypixel.hytale.plugin.early.ClassTransformer`.
*   **Loader**: Java ServiceLoader (Pre-Game).

---

## 2. Environment & Pathing

### Filesystem Layout
- **Root**: `/`
- **Server Jar**: `HytaleServer.jar` (Provided dependency)
- **Mod Source**: `src/main/java/`
- **Mod Resources**: `src/main/resources/`
- **Output**: `mods/my-mod.jar` (Game Plugins) or `earlyplugins/` (Early Plugins)

### Asset Paths (Standardized)
- `Server/Item/Items/` (Items)
- `Server/Item/Block/Blocks/` (Blocks)
- `Common/Models/item/` (Models)
- `Common/Textures/item/` (Textures)

## 3. Java Environment
- **Java Version**: Java 21+
- **Key Libs**: 
    - `it.unimi.dsi.fastutil` (Primitive collections preferred)
    - `com.google.gson` (JSON handling)
    - `io.netty` (Networking)
    - `org.slf4j` (Logging)

## 4. Core Patterns

### A. Game Plugin Entry Point
```java
public class MyMod extends JavaPlugin {
    public MyMod(JavaPluginInit init) {
        super(init);
    }
    
    @Override
    public void setup() {
        // Register Content Here
    }
    
    @Override
    public void start() {
        // Post-Init: Register Events & Commands
    }
    
    @Override
    public void shutdown() {
        // Cleanup
    }
}
```

### B. Registry Pattern (Event-Based)
Most systems use `EventRegistry` to register content during specific lifecycle events.

```java
server.getEventRegistry().register(RegisterAssetsEvent.class, e -> {
    e.registerItem(new MyItem());
});
```

### C. Singleton Access
- `HytaleServer.getInstance()` -> Global Server instance
- `EntityManager` -> via `server.getEntityManager()`
- `PacketRegistry` -> via `server.getPacketRegistry()`
- `PluginManager.get()` -> Plugin management
- `AnchorActionModule.get()` -> Anchor UI registration (Update 3)

## 5. Key Class Hierarchies (Stub)

### Entity
`Entity` -> `PhysicsEntity` -> `LivingEntity` -> `PlayerEntity`
                                 -> `MobEntity` -> `MonsterEntity`

### Item
`ItemBase` -> `ItemWeapon`
           -> `ItemTool`
           -> `ItemArmor`

### Block
`BlockType` (Definition) vs `BlockState` (Runtime instance)

## 6. Packet Construction
Packet classes MUST implement `com.hypixel.hytale.protocol.Packet`.
Required methods:
- `getId()`: unique int
- `serialize(ByteBuf)`
- `computeSize()`
