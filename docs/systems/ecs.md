# Entity Component System (ECS)

> **CONTEXT**: Documentation for Hytale's core ECS architecture.
> **SOURCE**: Analysis of `EntityStore`, `Component`, `RefSystem`, `Store`.

## 1. Overview

Hytale uses a custom Entity Component System (ECS) to manage game objects.
This system is performance-oriented and separates **Data** (Components) from **Logic** (Systems).

**Key Classes:**
*   `com.hypixel.hytale.server.core.universe.world.storage.EntityStore`: The world-level container for all entities.
*   `com.hypixel.hytale.component.Component`: Interface for data containers.
*   `com.hypixel.hytale.component.system.RefSystem`: Base class for logic that processes entities.
*   `com.hypixel.hytale.component.Ref<EntityStore>`: A handle to a specific entity.

## 2. Components

Components are pure data containers. Must implement `Component<EntityStore>`.
Should not contain game logic.

```java
public class MyComponent implements Component<EntityStore> {
    public int health;
    public String name;
    
    // Static Type accessor for queries
    public static final ComponentType<MyComponent> TYPE = ...; 
}
```

### B. System (`RefSystem`)
Contains the logic. Systems are registered to the `EntityStore` and update entities that match a specific `Query`.

```java
public class HealthSystem extends RefSystem<EntityStore> {
    
    @Override
    public Query<EntityStore> getQuery() {
        // Run on all entities with MyComponent and TransformComponent
        return Query.all(MyComponent.TYPE, TransformComponent.TYPE);
    }
    
    @Override
    public void update(Ref<EntityStore> ref, Store<EntityStore> store) {
        // Logic here
    }
}
```

### C. EntityStore
The `EntityStore` manages all entities and their components.
An "Entity" in the ECS is just an ID. The `Ref<EntityStore>` object is a lightweight handle used to access components associated with that ID. The `Entity` class (see [entities.md](entities.md)) is a high-level wrapper around this `Ref`.

## 3. Working with ECS

### Accessing Components
You typically interact with the `EntityStore` (available via `world.getEntityStore()`).

```java
// Get the store
Store<EntityStore> store = world.getEntityStore().getStore();

// Get a component from an entity reference
TransformComponent transform = store.getComponent(entityRef, TransformComponent.TYPE);

// Modify component
transform.position.y += 1.0;
```

### Adding/Removing Components

```java
// Add a component
store.addComponent(entityRef, new MyComponent(100));

// Remove a component
store.removeComponent(entityRef, MyComponent.TYPE);
```

### Creating Entities
Usually handled via `Entity` wrappers or `EntityFactory`, but low-level creation:

```java
// Create new entity ID
Ref<EntityStore> newEntity = store.createEntity();

// Add components
store.addComponent(newEntity, new TransformComponent(...));
```

## 4. Systems Registration

Systems must be registered to the specific `SystemGroup` within the `EntityStore` or `World`.
(See `EntityStore.java` static blocks or `World` initialization).
