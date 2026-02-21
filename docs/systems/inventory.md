# Inventory System

> **Status**: Verified against Decompiled Server Code (February 2026).
> **Source**: `com.hypixel.hytale.server.core.inventory.Inventory`

The Inventory system manages items for Players and LivingEntities. It consists of multiple **ItemContainers** (Hotbar, Storage, Armor, etc.).

## 1. Accessing Inventory

```java
Player player = ...;
Inventory inv = player.getInventory();

// Access specific containers
ItemContainer hotbar = inv.getHotbar();
ItemContainer storage = inv.getStorage();
ItemContainer armor = inv.getArmor();
```

## 2. Inventory Sections

| Section | ID | Capacity | Description |
|---------|----|----------|-------------|
| **Hotbar** | `-1` | 9 | The main action bar |
| **Storage** | `-2` | 36 | Main inventory storage |
| **Armor** | `-3` | 4 | Armor slots |
| **Utility** | `-5` | 4 | Offhand/Utility slots |
| **Tools** | `-8` | 23 | Tool belt (internal) |
| **Backpack** | `-9` | Dynamic | Optional backpack storage |

## 3. Modifying Items

### Adding Items
```java
ItemStack sword = new ItemStack(ItemManager.get("hytale:iron_sword"));
inv.getCombinedEverything().addItemStack(sword);
```

### Moving Items
```java
// Move 1 item from Storage (Slot 0) to Hotbar (Slot 0)
inv.moveItem(
    Inventory.STORAGE_SECTION_ID, 0, // From
    1,                               // Quantity
    Inventory.HOTBAR_SECTION_ID, 0   // To
);
```

### Checking for Items
```java
if (inv.getCombinedEverything().contains(ItemManager.get("hytale:stone"))) {
    // Player has stone
}
```

## 4. Inventory Events

Track changes to any inventory:

```java
this.getEventRegistry().register(LivingEntityInventoryChangeEvent.class, event -> {
    LivingEntity entity = event.getEntity();
    if (entity instanceof Player) {
        // Log changes
        System.out.println("Inventory changed for " + entity.getName());
    }
});
```

## 5. Dropping Items

To drop all items (e.g., on death):

```java
List<ItemStack> drops = inv.dropAllItemStacks();
// Spawn dropped items in world...
```
