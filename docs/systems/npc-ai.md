# NPC & AI System Specification

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.server.npc` and `com.hypixel.hytale.server.core.entity`.
> **SOURCE**: Decompiled from `HytaleServer.jar`.

## 1. Class Definition (`Entity`)

Target Class: `com.hypixel.hytale.server.core.entity.Entity`
Base class for all entities (Players, NPCs, Items, etc.).

```java
public abstract class Entity implements Component<EntityStore> {
    // --- Core Identity ---
    protected int networkId;                   // Runtime ID (synced)
    @Nullable protected UUID legacyUuid;       // Persistent UUID
    @Nullable protected World world;           // Current world ref
    @Nullable protected Ref<EntityStore> reference; // ECS Reference

    // --- State ---
    protected final AtomicBoolean wasRemoved;
    @Deprecated public TransformComponent transformComponent; // Use ECS!

    // --- Methods ---
    public void loadIntoWorld(World world);
    public boolean remove();
    public Holder<EntityStore> toHolder(); // Serializes entity to ECS Holder
}
```

## 2. NPC Architecture (`NPCPlugin`)

NPCs are built using a **Component-Entity-System (ECS)** and a **Behavior Tree** (GOAP-like) logic via `NPCPlugin`.

### Core Registries (Builders)
The `BuilderManager` in `NPCPlugin` handles AI logic registration.

| Category | Helper / Purpose | Factory Class |
|---|---|---|
| **Role** | High-level NPC type definition | `Role` |
| **Action** | Atomic behavior (e.g., Attack, Eat) | `Action` |
| **Sensor** | Input processing (e.g., CanSeePlayer) | `Sensor` |
| **Instruction** | Low-level command | `Instruction` |
| **BodyMotion** | Movement logic (e.g., Wander) | `BodyMotion` |
| **HeadMotion** | Looking logic (e.g., Watch) | `HeadMotion` |

### Standard AI Components (Registered in `NPCPlugin`)
*   **Sensors**: `Player`, `Mob`, `Damage`, `Light`, `Time`, `Block`, `Path`, `Sound`
*   **Actions**: `Attack`, `MoveTo`, `Flee`, `Eat`, `Sleep`, `Talk`
*   **Motions**: `Wander`, `WanderInCircle`, `Seek`, `Flee`, `MatchLook`

## 3. Entity Components (ECS)

Entities are composed of modular data components.

*   `TransformComponent`: Position, Rotation, Scale.
*   `VelocityComponent`: Physics velocity.
*   `MovementStatesComponent`: Flying, Swimming, Sprinting flags.
*   `DisplayNameComponent`: Name tag.
*   `ModelComponent`: Render model (`.blockymodel`).
*   `HealthComponent` / `DamageComponent`: Combat stats.

## 4. NPC JSON Schema (Role)

### Definition

AI/NPCs werden über JSON-Dateien in `Server/AI/Roles/` definiert.

```json
{
  "Name": "ExampleGuard",
  "Sensors": ["Player", "Damage"],
  "Actions": ["Attack", "MoveTo", "Talk"],
  "BodyMotion": "Wander",
  "HeadMotion": "MatchLook"
}
```
