# Entities (High-Level)

> **CONTEXT**: Documentation for `Entity` wrapper class and custom entities.
> **SOURCE**: Analysis of `Entity.java`, `LivingEntity.java`.

## 1. The Entity Wrapper

While Hytale uses an ECS, the `com.hypixel.hytale.server.core.entity.Entity` class provides a high-level Object-Oriented wrapper around the `Ref<EntityStore>`.

**Purpose:**
*   Easier API for common tasks (teleporting, damaging).
*   Manages lifecycle (`loadIntoWorld`, `remove`).
*   Bridge to the low-level `EntityStore`.

## 2. Key Methods

### Lifecycle
*   `loadIntoWorld(World)`: Initializes the entity in a world.
*   `remove()`: Despawns the entity and cleans up ECS components.

### Component Access
The `Entity` class provides helpers to access common components:
```java
// Get Transform (Position/Rotation)
TransformComponent transform = entity.getTransformComponent();

// Check if valid
if (entity.wasRemoved()) { ... }
```

## 3. Creating Custom Entities

To create a custom entity type, you typically extend `Entity` (or `LivingEntity`, `Mob`) and register it.

```java
public class MyCustomZombie extends LivingEntity {
    
    public MyCustomZombie() {
        super();
        // Register default components here
    }

    @Override
    public void loadIntoWorld(World world) {
        super.loadIntoWorld(world);
        // Add specific AI or components
    }
}
```

## 4. Entity Types & Registration

Entities are registered in the `EntityRegistry` (or similar, depending on version).
They often have an associated `EntityType` asset (JSON) defining their model, hitbox, and default components.

### Example Asset (`Server/Entity/Types/my_zombie.json`)
```json
{
  "Name": "my_mod:zombie",
  "Model": "Models/Creatures/zombie.blockymodel",
  "Components": [
    "Health",
    "Movement"
  ]
}
```
