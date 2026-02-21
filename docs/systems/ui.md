# User Interface (UI) Specification

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.server.core.ui.builder.UICommandBuilder`.
> **SOURCE**: Decompiled `UICommandBuilder.java`.

## 1. System Overview

Hytale's UI system allows servers to push **Custom Pages** to clients.
These pages are manipulated via `UICommandBuilder`, which queues commands (Set, Append, Remove) to be executed on the client's DOM-like UI tree.

**Target Class**: `com.hypixel.hytale.server.core.entity.entities.player.pages.CustomUIPage`

## 2. API Reference (`UICommandBuilder`)

The builder provides a fluent API to manipulate UI elements identified by a **selector** (string ID).


| Method | Description |
|--------|-------------|
| `append(String selector, String hxmlPath)` | Appends a .hxml file as a child. |
| `appendInline(String selector, String xml)` | Appends raw XML string as a child. |
| `insertBefore(String selector, String hxmlPath)` | Inserts .hxml before the element. |

### C. Complex Types

The builder supports specific Hytale UI types via `setObject(selector, object)` or overrides:

*   **`ItemStack`**: Displays an item.
*   **`Message`**: Server-side translated text.
*   **`Area`**: Defines a 2D region.
*   **`ItemGridSlot`**: Content for a grid slot.
*   **`LocalizableString`**: Client-side translation.
*   **`PatchStyle`**: 9-slice texture configuration.

## 3. Implementation Example

```java
public class MyCustomUI extends CustomUIPage {
    
    public MyCustomUI(PlayerRef player) {
        super(player, CustomPageLifetime.PAGED); 
    }

    @Override
    public void build(Ref<EntityStore> ref, UICommandBuilder cmd, UIEventBuilder evt, Store<EntityStore> store) {
        // Load base layout
        cmd.append("#root", "my_mod:uis/main_menu.hxml");

        // Set dynamic text
        cmd.set("title_text", "Welcome to Hytale!");
        cmd.set("coins_count", 150);

        // Show an Item
        ItemStack sword = new ItemStack(ItemRegistry.getItem("hytale:iron_sword"));
        cmd.setObject("reward_icon", sword);

        // Bind click event via UIEventBuilder
        evt.register("btn_claim", "click", (eventData) -> {
            // Handle claim reward
        });
    }
}
```

## 4. Key Classes
- `UICommandBuilder`: Fluent API for adding text, images, buttons. Pfad: `server/core/ui/builder/UICommandBuilder.java`
- `UIEventBuilder`: Binds interaction events (clicks) to server-side callbacks. Pfad: `server/core/ui/builder/UIEventBuilder.java`
- `CustomPageLifetime`: Defines if UI closes on Escape (`PAGED`) or stays (`PERSISTENT`).

---

## 5. Anchor UI System (Update 3)

> **NEU in Update 3**: Plugins können UI-Elemente in vordefinierte "Anchor Points" auf Client-Seiten einfügen (z.B. World Map).

### Architektur

```
AnchorActionModule (server/core/modules/anchoraction/)
├── AnchorActionHandler.java    @FunctionalInterface
├── AnchorActionModule.java     Singleton JavaPlugin
└── register(action, handler)   Registration API

UpdateAnchorUI (protocol/packets/interface_/)
├── Packet ID: 235
├── anchorId: String           Anchor-Punkt-Bezeichner
├── clear: boolean             Vorhandene UI löschen
├── commands: CustomUICommand[] UI-Befehle
└── eventBindings: CustomUIEventBinding[]  Event-Bindings
```

### API

```java
// Zugriff auf den Singleton
AnchorActionModule module = AnchorActionModule.get();

// Action registrieren
module.register("my_action", (PlayerRef player, JsonObject data) -> {
    String actionType = data.get("action").getAsString();
    // Handle action from client UI
});

// World-Thread-sichere Variante (automatische World-Thread-Synchronisation)
module.register("my_world_action", (PlayerRef player, Ref<EntityStore> ref, Store<EntityStore> store, JsonObject data) -> {
    // Sicher auf Welt-Daten zugreifen
});

// Action entfernen
module.unregister("my_action");
```

### Anchor Layout-Klasse

Die `Anchor`-Klasse (`server/core/ui/Anchor.java`) definiert Layout-Eigenschaften:

| Property | Typ | Beschreibung |
|----------|-----|-------------|
| `Left` | `Value<Integer>` | Abstand links |
| `Right` | `Value<Integer>` | Abstand rechts |
| `Top` | `Value<Integer>` | Abstand oben |
| `Bottom` | `Value<Integer>` | Abstand unten |
| `Width` | `Value<Integer>` | Breite |
| `Height` | `Value<Integer>` | Höhe |
| `MinWidth` | `Value<Integer>` | Mindestbreite |
| `MaxWidth` | `Value<Integer>` | Maximalbreite |
| `Horizontal` | `Value<Integer>` | Horizontale Ausrichtung |
| `Vertical` | `Value<Integer>` | Vertikale Ausrichtung |
| `Full` | `Value<Integer>` | Volle Ausdehnung |

### Weitere UI-Klassen (`server/core/ui/`)

| Klasse | Beschreibung |
|--------|-------------|
| `Anchor` | Layout-Positioning |
| `Area` | 2D-Region-Definition |
| `Value` / `ValueCodec` | Generische Wert-Container |
| `LocalizableString` | Übersetzbare Strings |
| `PatchStyle` | 9-Slice Textur-Konfiguration |
| `ItemGridSlot` | Grid-Slot Inhalt |
| `DropdownEntryInfo` | Dropdown-Einträge |
