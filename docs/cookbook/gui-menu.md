# Cookbook: Custom UI Page

This pattern demonstrates how to create a custom UI page using the verified `CustomUIPage` API with `UICommandBuilder` and `UIEventBuilder`.

> **See Also**: [UI System Reference](../systems/ui.md)

## 1. The HXML Layout

UI layouts are defined in `.hxml` files inside your asset pack.

**File**: `assets/my_mod/uis/welcome_menu.hxml`

```xml
<panel id="root">
    <text id="title_text">Welcome</text>
    <text id="player_name"></text>
    <button id="btn_action">Click Me</button>
</panel>
```

## 2. The CustomUIPage

Extend `CustomUIPage` to build and populate the UI server-side.

**File**: `src/main/java/com/example/ui/WelcomeMenu.java`

```java
package com.example.ui;

import com.hypixel.hytale.component.Ref;
import com.hypixel.hytale.component.Store;
import com.hypixel.hytale.server.core.universe.PlayerRef;
import com.hypixel.hytale.server.core.entity.entities.player.pages.CustomUIPage;
import com.hypixel.hytale.protocol.packets.interface_.CustomPageLifetime;
import com.hypixel.hytale.server.core.ui.builder.UICommandBuilder;
import com.hypixel.hytale.server.core.ui.builder.UIEventBuilder;
import com.hypixel.hytale.server.core.universe.world.storage.EntityStore;

public class WelcomeMenu extends CustomUIPage {

    public WelcomeMenu(PlayerRef player) {
        super(player, CustomPageLifetime.PAGED); // Closes on Escape
    }

    @Override
    public void build(Ref<EntityStore> ref, UICommandBuilder cmd, UIEventBuilder evt, Store<EntityStore> store) {
        // Load the HXML layout
        cmd.append("#root", "my_mod:uis/welcome_menu.hxml");

        // Set dynamic text content
        cmd.set("title_text", "Welcome to the Server!");
        cmd.set("player_name", "Hello, Player!");

        // Register a click handler via UIEventBuilder
        // The event ID is used to route callbacks server-side
        evt.register("btn_action", "click", (eventData) -> {
            getLogger().info("Button clicked!");
        });
    }
}
```

## 3. The Plugin

**File**: `src/main/java/com/example/ui/UIPlugin.java`

```java
package com.example.ui;

import com.hypixel.hytale.server.core.plugin.JavaPlugin;
import com.hypixel.hytale.server.core.plugin.JavaPluginInit;
import com.hypixel.hytale.server.core.event.events.player.PlayerConnectEvent;

public class UIPlugin extends JavaPlugin {

    public UIPlugin(JavaPluginInit init) {
        super(init);
    }

    @Override
    public void start() {
        // Open the welcome menu when a player connects
        this.getEventRegistry().register(PlayerConnectEvent.class, event -> {
            WelcomeMenu menu = new WelcomeMenu(event.getPlayerRef());
            // The page is registered with the player's page manager
        });
    }
}
```

## 4. Key Takeaways

1.  **`CustomUIPage`**: Extend this class to define custom UIs. Constructor takes `(PlayerRef, CustomPageLifetime)`.
2.  **`UICommandBuilder`**: Use `append(selector, hxmlPath)` to load layouts, `set(selector, value)` for dynamic content, `appendInline(selector, xml)` for inline XML.
3.  **`UIEventBuilder`**: Bind client-side events (clicks, etc.) to server-side callbacks.
4.  **`CustomPageLifetime`**: `PAGED` (closes on Escape) or `PERSISTENT` (stays open).
5.  **Layouts**: Define structure in `.hxml` files, populate/manipulate server-side via the builder.

---

## 5. Anchor UI System (Update 3)

> **NEU in Update 3**: Neben CustomUIPage gibt es jetzt das **Anchor UI System** — ein leichtgewichtiger Weg, UI-Elemente in *bestehende* Client-Seiten (z.B. World Map, HUD) einzufügen, ohne eine eigene Vollseite zu öffnen.

### Wann welches System?

| | `CustomUIPage` | Anchor UI |
|---|---|---|
| **Use Case** | Eigene Vollbild-Menüs, Shops, GUIs | UI-Elemente in bestehende Seiten injizieren |
| **Sichtbarkeit** | Öffnet eine neue Seite | Ergänzt vorhandene Client-UI |
| **Lifecycle** | PAGED (Escape schließt) oder PERSISTENT | Vom Server gesteuert (clear/update) |
| **Kommunikation** | UIEventBuilder Callbacks | JSON-basierte Actions |

### Beispiel: Custom Map-Marker via Anchor UI

```java
package com.example.anchordemo;

import com.hypixel.hytale.server.core.plugin.JavaPlugin;
import com.hypixel.hytale.server.core.plugin.JavaPluginInit;
import com.hypixel.hytale.server.core.modules.anchoraction.AnchorActionModule;
import com.hypixel.hytale.logger.HytaleLogger;

public class AnchorDemoPlugin extends JavaPlugin {

    public AnchorDemoPlugin(JavaPluginInit init) {
        super(init);
    }

    @Override
    public void start() {
        AnchorActionModule anchors = AnchorActionModule.get();
        HytaleLogger log = this.getLogger();

        // Einfacher Handler: Empfängt Client-Actions (z.B. Button-Klick in einem Anchor)
        anchors.register("marker_click", (player, data) -> {
            String markerId = data.get("markerId").getAsString();
            log.info("Spieler " + player.getUuid() + " klickte Marker: " + markerId);
        });

        // World-Thread-sicherer Handler: Zugriff auf ECS/Welt-Daten
        anchors.register("teleport_to_marker", (player, ref, store, data) -> {
            // Hier ist man sicher im World-Thread
            double x = data.get("x").getAsDouble();
            double z = data.get("z").getAsDouble();
            log.info("Teleport zu " + x + ", " + z);
            // Entity-Manipulation hier sicher möglich
        });
    }

    @Override
    public void shutdown() {
        // Eigene Actions aufräumen
        AnchorActionModule anchors = AnchorActionModule.get();
        anchors.unregister("marker_click");
        anchors.unregister("teleport_to_marker");
    }
}
```

### Wie der Client kommuniziert

Der Client sendet JSON-Daten an den Server. Das `AnchorActionModule` dispatcht anhand des `action`-Felds:

```json
{
  "action": "marker_click",
  "markerId": "quest_npc_01",
  "x": 150.5,
  "z": -230.0
}
```

Der Server antwortet mit `UpdateAnchorUI`-Paketen (Packet 235), um UI in Client-Anchor-Punkte einzufügen.

> **See Also**: [UI System Reference — Anchor UI](../systems/ui.md#5-anchor-ui-system-update-3)

