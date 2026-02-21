# Hytale Modding Best Practices

## 1. Lifecycle Management
*   **`preLoad()`** (Update 3): Wird vor `setup()` aufgerufen – für frühe Initialisierung (z.B. CodecRegistry). Gibt `CompletableFuture` zurück, alle Plugins werden parallel pre-loaded.
*   **`setup()`**: Configs laden, interne Zustände initialisieren, Registries befüllen.
*   **`start()`**: **Events**, Commands, Tasks registrieren. Interaktion mit anderen Plugins.
*   **`shutdown()`**: Ressourcen aufräumen, Zustände speichern und **Anchor UI Actions deregistrieren** (`AnchorActionModule.get().unregister()`).

## 2. Logging
*   ❌ **Nicht** `System.out.println()` oder `org.slf4j.Logger` verwenden.
*   ✅ **`this.getLogger()`** gibt `HytaleLogger` zurück (`com.hypixel.hytale.logger.HytaleLogger`).

## 3. Performance
*   **Event Handlers**: Logik minimal halten. Schwere Aufgaben in separate Threads auslagern, aber Hytale-API (Entities, Blocks) **nicht** aus Async-Threads aufrufen.
*   **Anchor UI**: `WorldThreadAnchorActionHandler` verwenden, wenn ECS-Zugriff nötig ist — automatische World-Thread-Synchronisation.
*   **Tasks**: `getTaskRegistry().registerTask()` für Lifecycle-Management von Async-Tasks nutzen (kein `runLater`/`runRepeating` — das gibt es nicht!).

## 4. Manifest (Update 3)
*   **`ServerVersion`**: Immer setzen! Der PluginManager warnt, wenn Plugin-Version nicht zur Server-Version passt.
*   **`SubPlugins`**: Für modulare Mods — Sub-Module im Manifest deklarieren.
*   **Dependencies**: Im Manifest deklarieren, um korrekte Ladereihenfolge sicherzustellen.

## 5. Asset Management
*   Immer `Server/` Verzeichnisstruktur für Assets verwenden (`Server/Item/Items/`).
*   JSON-Definitionen gegen `docs/SCHEMA/` validieren.

## 6. Kompatibilität
*   Eindeutige Namespaces für Items/Blocks verwenden (`my_mod:item_name`).
*   Plugin-State `FAILED` abfangen: Plugins die Fehler werfen, werden in den `FAILED`-State versetzt und blockieren nicht den gesamten Server.

