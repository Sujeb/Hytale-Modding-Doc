# Reguläres Plugin-System

> **WICHTIG**: Dies ist das Haupt-Plugin-System für ~95% aller Mods!
> Early Plugins sind nur für Bytecode-Modifikation.

---

## Unterschied: Regular vs Early Plugins

| Aspekt | Regular Plugins | Early Plugins |
|--------|-----------------|---------------|
| **Verwendung** | 95% aller Mods | Bytecode-Modifikation |
| **Basisklasse** | `JavaPlugin` | `ClassTransformer` |
| **Verzeichnis** | `mods/` | `earlyplugins/` |
| **Manifest** | `manifest.json` | ServiceLoader |
| **API-Zugriff** | Volle API (Events, Commands, etc.) | Nur Transform |
| **Lifecycle** | setup() → start() → shutdown() | transform() bei Class-Load |

---

## Quick Start

### 1. Das Manifest (`manifest.json`)

Jedes Plugin muss eine `manifest.json` Datei im Root-Verzeichnis enthalten.
Diese Datei definiert Metadaten und den Einstiegspunkt.

### Beispiel
```json
{
  "Name": "MyPlugin",
  "Group": "com.example",
  "Version": "1.0.0",
  "Main": "com.example.MyPlugin",
  "ServerVersion": "1.0.0"
}
```

## 2. Struktur

Plugins werden als `.jar` Dateien in den `mods/` Ordner geladen.

```
server/
├── mods/
│   └── my-plugin.jar
├── HytaleServer.jar
└── start.sh
```

## 3. Der Lebenszyklus (`PluginBase`)

Die Hauptklasse muss `JavaPlugin` (erbt von `PluginBase`) erweitern.

```java
import com.hypixel.hytale.server.core.plugin.JavaPlugin;
import com.hypixel.hytale.server.core.plugin.JavaPluginInit;
import com.hypixel.hytale.server.core.event.events.player.PlayerConnectEvent;

public class MyPlugin extends JavaPlugin {

    public MyPlugin(JavaPluginInit init) {
        super(init);
    }

    @Override
    public void setup() {
        // Initialisierung
    }

    @Override
    public void start() {
        // Event-Registrierung
        this.getEventRegistry().register(PlayerConnectEvent.class, event -> {
            // ...
        });
    }

    @Override
    public void shutdown() {
        // Cleanup
    }
}
```
### 1. Plugin-Klasse

```java
package com.meinmod;

import com.hypixel.hytale.server.core.plugin.JavaPlugin;
import com.hypixel.hytale.server.core.plugin.JavaPluginInit;

public class MeinPlugin extends JavaPlugin {
    
    public MeinPlugin(JavaPluginInit init) {
        super(init);
    }
    
    @Override
    protected void setup() {
        // Registrierungen VOR dem Start
        getLogger().info("Setup...");
    }
    
    @Override
    protected void start() {
        // Plugin aktiv
        getLogger().info("Gestartet!");
        
        // Events registrieren
        getEventRegistry().register(MyEvent.class, this::onMyEvent);
        
        // Commands registrieren
        getCommandRegistry().register(new MyCommand());
    }
    
    @Override
    protected void shutdown() {
        // Cleanup
        getLogger().info("Heruntergefahren!");
    }
    
    private void onMyEvent(MyEvent event) {
        // Event handling
    }
}
```

### 2. manifest.json (Manifest)

```json
{
    "Group": "com.meinmod",
    "Name": "MeinPlugin",
    "Version": "1.0.0",
    "Description": "Mein erstes Plugin",
    "Authors": [
        {
            "Name": "MeinName",
            "Role": "Developer"
        }
    ],
    "Website": "https://example.com",
    "Main": "com.meinmod.MeinPlugin",
    "ServerVersion": ">=1.0.0",
    "Dependencies": {
        "Hytale:CorePlugin": "*"
    },
    "OptionalDependencies": {},
    "LoadBefore": {},
    "DisabledByDefault": false,
    "IncludesAssetPack": false
}
```

### 3. Deployment

```bash
# JAR mit manifest.json im Root erstellen
jar cvf mein-plugin.jar -C target/classes .

# In mods/ Verzeichnis kopieren
cp mein-plugin.jar /pfad/zum/server/mods/
```

---

## PluginBase API

Jedes Plugin erbt diese eingebauten Registries:

```java
// Pfad: server/core/plugin/PluginBase.java (318 Zeilen, 12KB)

public abstract class PluginBase implements CommandOwner {
    
    // === Logging ===
    HytaleLogger getLogger();
    
    // === Identifikation ===
    PluginIdentifier getIdentifier();
    PluginManifest getManifest();
    Path getDataDirectory();
    PluginState getState();
    boolean isEnabled();
    boolean isDisabled();
    String getBasePermission();
    
    // === Eingebaute Registries ===
    EventRegistry getEventRegistry();
    CommandRegistry getCommandRegistry();
    EntityRegistry getEntityRegistry();
    TaskRegistry getTaskRegistry();
    AssetRegistry getAssetRegistry();
    BlockStateRegistry getBlockStateRegistry();
    ClientFeatureRegistry getClientFeatureRegistry();
    
    // === ECS Proxies ===
    ComponentRegistryProxy<EntityStore> getEntityStoreRegistry();
    ComponentRegistryProxy<ChunkStore> getChunkStoreRegistry();
    
    // === Codec Registries (Update 3) ===
    <T, C extends Codec<? extends T>> CodecMapRegistry<T, C> getCodecRegistry(StringCodecMapCodec<T, C> mapCodec);
    <K, T extends JsonAsset<K>> CodecMapRegistry.Assets<T, ?> getCodecRegistry(AssetCodecMapCodec<K, T> mapCodec);
    <V> MapKeyMapRegistry<V> getCodecRegistry(MapKeyMapCodec<V> mapCodec);
    
    // === Config ===
    <T> Config<T> withConfig(BuilderCodec<T> codec);
    <T> Config<T> withConfig(String name, BuilderCodec<T> codec);
    CompletableFuture<Void> preLoad();  // Async Config-Laden
    
    // === Lifecycle (überschreiben) ===
    protected void setup();    // Vor Start
    protected void start();    // Plugin aktiv
    protected void shutdown(); // Beim Beenden
}
```

---

## Plugin-Lifecycle

```
1. NONE        → Plugin geladen
2. SETUP       → setup() aufgerufen
3. START       → start() aufgerufen
4. ENABLED     → Plugin läuft
5. SHUTDOWN    → shutdown() aufgerufen
6. DISABLED    → Plugin deaktiviert
7. FAILED      → Fehler bei setup/start/shutdown
```

### Lifecycle-Methoden

```java
@Override
protected void setup() {
    // Wird VOR start() aufgerufen
    // Hier: Configs laden, Registrierungen vorbereiten
    // NICHT: Aktive Logik
}

@Override
protected void start() {
    // Plugin ist jetzt aktiv
    // Hier: Events registrieren, Commands aktivieren
    
    getEventRegistry().register(PlayerConnectEvent.class, event -> {
        getLogger().info("Spieler beigetreten: " + event.getPlayer());
    });
}

@Override
protected void shutdown() {
    // Server fährt herunter oder Plugin wird deaktiviert
    // Hier: Cleanup, Daten speichern
    // Registrierungen werden automatisch aufgeräumt!
}
```

---

## Manifest-Felder

| Feld | Typ | Pflicht | Beschreibung |
|------|-----|---------|--------------|
| `Group` | String | ✓ | Package-Gruppe (z.B. "com.meinmod") |
| `Name` | String | ✓ | Plugin-Name |
| `Version` | Semver | ✓ | Version (z.B. "1.0.0") |
| `Description` | String | | Beschreibung |
| `Authors` | Array | | Autoren mit Name und Role |
| `Website` | String | | Website-URL |
| `Main` | String | ✓ | Hauptklasse (FQCN) |
| `ServerVersion` | String | | Kompatible Server-Version (Validierung seit Update 3) |
| `Dependencies` | Map | | Abhängigkeiten (PluginId → VersionRange) |
| `OptionalDependencies` | Map | | Optionale Abhängigkeiten |
| `LoadBefore` | Map | | Vor diesen Plugins laden |
| `SubPlugins` | Array | | Verschachtelte Sub-Plugin-Manifeste (Update 3) |
| `DisabledByDefault` | Boolean | | Standardmäßig deaktiviert |
| `IncludesAssetPack` | Boolean | | Enthält Asset-Pack |

---

## Event-Registration

```java
// In start():

// Einfach
getEventRegistry().register(MyEvent.class, this::handleEvent);

// Mit Priorität
getEventRegistry().register(EventPriority.EARLY, MyEvent.class, event -> {
    // früh ausführen
});

// Async
getEventRegistry().registerAsync(AsyncEvent.class, future -> {
    return future.thenApply(event -> {
        // async verarbeiten
        return event;
    });
});
```

---

## Command-Registration

```java
// In start():
getCommandRegistry().register(new MyCommand());
```

---

## Entity-Registration

```java
// Eigene Entity-Typen
getEntityRegistry().register(MyEntityType.class, MyEntity::new);
```

---

## Task-Registration

```java
// Task beim Plugin-System registrieren (Java ScheduledFuture)
ScheduledExecutorService executor = Executors.newSingleThreadScheduledExecutor();
ScheduledFuture<?> task = executor.scheduleAtFixedRate(
    () -> { /* periodische Arbeit */ },
    0, 1, TimeUnit.SECONDS
);
getTaskRegistry().registerTask(task);
```

---

## Config-System

```java
public class MeinPlugin extends JavaPlugin {
    private final Config<MeinConfig> config;
    
    public MeinPlugin(JavaPluginInit init) {
        super(init);
        // Muss im Konstruktor sein!
        this.config = withConfig(MeinConfig.CODEC);
    }
    
    @Override
    protected void start() {
        MeinConfig cfg = config.get();
        getLogger().info("Config: " + cfg.getSomeValue());
    }
}
```

---

## Asset-Packs

Plugins können eigene Assets mitbringen:

```json
{
    "IncludesAssetPack": true
}
```

Assets werden aus dem JAR geladen.

---

## Permissions

```java
// Base Permission automatisch: "group.name"
String basePerm = getBasePermission();
// z.B. "com.meinmod.meinplugin"

// Für Commands etc.
String cmdPerm = getBasePermission() + ".command.mycommand";
```

---

## Wichtige Pfade

```
Reguläre Plugins:
decompiled/com/hypixel/hytale/server/core/plugin/

PluginManager.java            60KB  Plugin-Verwaltung
PluginBase.java               12KB  Basis-Klasse (318 Zeilen)
JavaPlugin.java                2KB  Java-Plugin-Klasse
PluginClassLoader.java         6KB  ClassLoader
PluginManifest.java           14KB  Manifest-Definition (common/plugin/)
PluginListPageManager.java     -    Plugin-Liste UI (Update 3)
```

---

## Best Practices

### DO
- ✅ `JavaPlugin` erweitern, nicht `PluginBase`
- ✅ Lifecycle-Methoden nutzen (setup, start, shutdown)
- ✅ Eingebaute Registries verwenden
- ✅ Dependencies im Manifest deklarieren
- ✅ Semantic Versioning nutzen

### DON'T
- ❌ Keine eigenen ClassLoader erstellen
- ❌ Keine statischen Initialisierungen mit Server-Abhängigkeiten
- ❌ Keine Events in setup() registrieren (erst in start())
- ❌ Keine harten Abhängigkeiten ohne Manifest-Eintrag
