# Event System Specification

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.event`.
> **SOURCE**: Decompiled `EventRegistry`, `EventPriority`, and `events` package.

## 1. System Overview

**Manager**: `com.hypixel.hytale.event.EventRegistry`
**Priority Enum**: `com.hypixel.hytale.event.EventPriority`

Events are strictly typed classes implementing `IEvent<T>`.

## 2. Registration Pattern

Plugins register listeners via their `getEventRegistry()`.

```java
import com.hypixel.hytale.server.core.event.events.player.PlayerConnectEvent;
import com.hypixel.hytale.server.core.event.events.ecs.BreakBlockEvent;

public void start() {
    this.getEventRegistry().register(PlayerConnectEvent.class, event -> {
        // Logic
        getLogger().info("Player connected: " + event.getPlayerRef());
    });
}
```


## 3. Full Verified Event List (34 Events)
 
 ### Lifecycle (`com.hypixel.hytale.server.core.event.events`)
 *   `BootEvent` - Server startup.
 *   `ShutdownEvent` - Server shutdown.
 *   `PrepareUniverseEvent` - World loading.
 
 ### Player (`com.hypixel.hytale.server.core.event.events.player`)
 *   `PlayerConnectEvent` - Joining.
 *   `PlayerDisconnectEvent` - Leaving.
 *   `PlayerReadyEvent` - Connection finalized.
 *   `PlayerChatEvent` - Chat message.
 *   `PlayerInteractEvent` - Interaction (Right-click).
 *   `PlayerCraftEvent` - Crafting.
 *   `PlayerMouseButtonEvent` - Mouse click.
 *   `PlayerMouseMotionEvent` - Mouse move.
 *   `AddPlayerToWorldEvent` - Player spawns in world.
 *   `DrainPlayerFromWorldEvent` - Player despawns from world.
 *   `PlayerSetupConnectEvent` - Early auth phase.
 *   `PlayerSetupDisconnectEvent` - Early auth disconnect.
 *   `PlayerEvent` - Base player event (Update 3).
 *   `PlayerRefEvent` - Player reference-based event (Update 3).
 
 ### Permissions (`com.hypixel.hytale.server.core.event.events.permissions`) *(Update 3: eigenes Subpackage)*
 *   `PlayerGroupEvent` - Group change.
 *   `PlayerPermissionChangeEvent` - Permission change.
 *   `GroupPermissionChangeEvent` - Group-level permission change (Update 3).

 ### ECS / Gameplay (`com.hypixel.hytale.server.core.event.events.ecs`)
 *   `BreakBlockEvent` - Block broken.
 *   `PlaceBlockEvent` - Block placed.
 *   `DamageBlockEvent` - Block damaged.
 *   `UseBlockEvent` - Block used.
 *   `CraftRecipeEvent` - Recipe result.
 *   `ChangeGameModeEvent` - Gamemode switch.
 *   `DiscoverZoneEvent` - Zone entry.
 *   `DropItemEvent` - Item dropped.
 *   `InteractivelyPickupItemEvent` - Pickup.
 *   `SwitchActiveSlotEvent` - Hotbar switch.
 
 ### Entity (`com.hypixel.hytale.server.core.event.events.entity`)
 *   `EntityEvent` - Base entity event.
 *   `EntityRemoveEvent` - Entity removal.
 *   `LivingEntityInventoryChangeEvent` - Inventory update.
 *   `LivingEntityUseBlockEvent` - Entity uses a block (Update 3).

## 4. Priorities

Handlers can have a priority. Lower values run first.

```java
this.getEventRegistry().register(
    EventPriority.FIRST, // FIRST(-21844), EARLY(-10922), NORMAL(0), LATE(10922), LAST(21844)
    BreakBlockEvent.class, 
    event -> { /* ... */ }
);
```
