# Hytale Modding - LLM Reference Index

> **Für Antigravity/LLM-Nutzung optimiert**
> Direkte Pfade, API-Signaturen, Lookup-Tabellen

---

## Quick Lookup: "Ich will X machen"

| Ziel | System | Relevante Dateien |
|------|--------|-------------------|
| **Plugin erstellen (Standard)** | Regular Plugin | `server/core/plugin/JavaPlugin.java` |
| Bytecode modifizieren | Early Plugin | `plugin/early/ClassTransformer.java` |
| Events abonnieren | Regular Plugin | `getEventRegistry().register()` |
| Commands hinzufügen | Regular Plugin | `getCommandRegistry().register()` |
| Entities registrieren | Regular Plugin | `getEntityRegistry().register()` |
| Tasks planen | Regular Plugin | `getTaskRegistry().registerTask()` |
| ECS-Komponenten | - | `component/Holder.java` |
| Neues Item | Protocol | `protocol/ItemBase.java` (100KB) |
| Neuen Block | Protocol | `protocol/BlockType.java` (102KB) |
| NPC-Verhalten | Server | `server/npc/NPCPlugin.java` (92KB) |
| Weltgenerierung | Server | `server/worldgen/` |
| Packets | Protocol | `protocol/Packet.java`, `PacketRegistry.java` |
| **Anchor UI registrieren** | Regular Plugin | `AnchorActionModule.get().register()` (Update 3) |

> **95% aller Mods verwenden Regular Plugins!**
> Early Plugins nur für Bytecode-Transformation.

---

## Plugin-System

### Regular Plugins (95% aller Mods)

```
Pfad: decompiled/com/hypixel/hytale/server/core/plugin/
Manifest: decompiled/com/hypixel/hytale/common/plugin/
```

| Datei | Größe | Beschreibung |
|-------|-------|--------------|
| `PluginManager.java` | 60KB | Plugin-Verwaltung |
| `PluginBase.java` | 12KB | Basis mit Registries |
| `JavaPlugin.java` | 2KB | Plugin-Klasse |
| `PluginManifest.java` | 14KB | Manifest-Definition |

**Lifecycle**: `Constructor(JavaPluginInit)` → `setup()` → `start()` → `shutdown()`

**Eingebaute Registries**:
```java
getEventRegistry()      // Events
getCommandRegistry()    // Commands
getEntityRegistry()     // Entities
getTaskRegistry()       // Scheduled Tasks
getAssetRegistry()      // Assets
getBlockStateRegistry() // Block States
getCodecRegistry(...)   // Codec Registries (Update 3)
```

### Early Plugins (Bytecode-Modifikation)
```
Pfad: decompiled/com/hypixel/hytale/plugin/early/
```

| Datei | Signatur | Zweck |
|-------|----------|-------|
| `ClassTransformer.java` | `interface ClassTransformer { int priority(); @Nullable byte[] transform(String, String, byte[]); }` | Bytecode-Transform |
| `EarlyPluginLoader.java` | `class EarlyPluginLoader { static void loadEarlyPlugins(String[]); }` | JAR-Loader |

---

## Event-System

### Pfad
```
decompiled/com/hypixel/hytale/event/
```

### Kern-Interfaces

| Interface | Signatur |
|-----------|----------|
| `IEvent<K>` | `interface IEvent<KeyType> extends IBaseEvent<KeyType>` |
| `IAsyncEvent<K>` | `interface IAsyncEvent<KeyType> extends IBaseEvent<KeyType>` |
| `ICancellable` | `boolean isCancelled(); void setCancelled(boolean);` |
| `IEventBus` | `<E extends IEvent<K>> E dispatch(Class<E>)` |
| `IEventRegistry` | `EventRegistration register(Class, Consumer)` |

### Event-Prioritäten
```java
FIRST = -21844
EARLY = -10922
NORMAL = 0
LATE = 10922
LAST = 21844
```

### Register-Pattern
```java
eventRegistry.register(EventClass.class, event -> { });
eventRegistry.register(EventPriority.EARLY, EventClass.class, handler);
eventRegistry.registerAsync(AsyncEventClass.class, future -> future.thenApply(...));
```

---

## ECS-System

### Pfad
```
decompiled/com/hypixel/hytale/component/
```

### Kern-Klassen

| Klasse | Größe | Wichtigste Methoden |
|--------|-------|---------------------|
| `Holder.java` | 23KB | `getComponent(type)`, `addComponent(type, comp)`, `removeComponent(type)`, `putComponent(type, comp)` |
| `Store.java` | 98KB | Entity-Speicher |
| `ComponentRegistry.java` | 72KB | `createComponent(type)`, `newHolder()` |
| `Archetype.java` | 12KB | `contains(type)`, `add()`, `remove()` |
| `Component.java` | 451B | `interface Component<T> extends Cloneable { Component<T> clone(); }` |

### Component-Zugriff
```java
// Lesen
MyComponent c = holder.getComponent(MY_COMPONENT_TYPE);

// Schreiben
holder.addComponent(MY_TYPE, new MyComponent());
holder.replaceComponent(MY_TYPE, newComponent);
holder.putComponent(MY_TYPE, component);  // add or replace

// Entfernen
holder.removeComponent(MY_TYPE);
```

---

## Protocol/Networking

### Pfad
```
decompiled/com/hypixel/hytale/protocol/
```

### Packet-Interface
```java
interface Packet {
    int getId();
    void serialize(ByteBuf buf);
    int computeSize();
}
```

### Wichtigste Dateien

| Datei | Größe | Inhalt |
|-------|-------|--------|
| `BlockType.java` | 102KB | Alle Block-Eigenschaften |
| `Weather.java` | 101KB | Wetter-System |
| `ItemBase.java` | 100KB | Alle Item-Eigenschaften |
| `PacketRegistry.java` | 85KB | Packet-Registrierung |
| `PlayerSkin.java` | 55KB | Skin-System |
| `ComponentUpdate.java` | 54KB | ECS-Synchronisation |
| `Model.java` | 49KB | Modell-Definitionen |

### Packet-Kategorien
```
decompiled/com/hypixel/hytale/protocol/packets/
├── assets/      # Asset-Updates
├── auth/        # Authentifizierung
├── camera/      # Kamera
├── connection/  # Verbindung
├── entities/    # Entity-Updates
├── interaction/ # Interaktionen
├── interface_/  # UI
├── inventory/   # Inventar
├── player/      # Spieler
├── world/       # Welt
└── ... (17 total)
```

---

## Items

### Haupt-Datei
```
decompiled/com/hypixel/hytale/protocol/ItemBase.java (100KB)
```

### Item-Typen
| Klasse | Pfad |
|--------|------|
| `ItemBase` | `protocol/ItemBase.java` |
| `ItemWeapon` | `protocol/ItemWeapon.java` |
| `ItemArmor` | `protocol/ItemArmor.java` |
| `ItemTool` | `protocol/ItemTool.java` |
| `ItemUtility` | `protocol/ItemUtility.java` |
| `ItemGlider` | `protocol/ItemGlider.java` |

---

## Blocks

### Haupt-Datei
```
decompiled/com/hypixel/hytale/protocol/BlockType.java (102KB)
```

### Block-Komponenten
| Klasse | Pfad | Größe |
|--------|------|-------|
| `BlockType` | `protocol/BlockType.java` | 102KB |
| `BlockTextures` | `protocol/BlockTextures.java` | 17KB |
| `BlockSoundSet` | `protocol/BlockSoundSet.java` | 10KB |
| `BlockParticleSet` | `protocol/BlockParticleSet.java` | 13KB |
| `BlockBreaking` | `protocol/BlockBreaking.java` | 11KB |

---

## NPC/AI

### Pfad
```
decompiled/com/hypixel/hytale/server/npc/
```

### Struktur
| Sub-Package | Zweck |
|-------------|-------|
| `blackboard/` | AI-Daten-Sharing |
| `decisionmaker/` | Entscheidungslogik |
| `instructions/` | AI-Befehle |
| `role/` | NPC-Rollen |
| `statetransition/` | State Machine |
| `movement/` | Bewegung |
| `navigation/` | Pathfinding |
| `sensorinfo/` | Umgebungswahrnehmung |
| `components/` | ECS-Komponenten |

---

## WorldGen

### Pfad
```
decompiled/com/hypixel/hytale/server/worldgen/
```

### Struktur
| Sub-Package | Zweck |
|-------------|-------|
| `biome/` | Biom-Definitionen |
| `climate/` | Klima-System |
| `cave/` | Höhlen |
| `prefab/` | Strukturen |
| `zone/` | Welt-Zonen |
| `chunk/` | Chunk-Generierung |
| `loader/` | Asset-Loader |

---

## Builtin-Module (32)

### Pfad
```
decompiled/com/hypixel/hytale/builtin/
```

### Liste
```
adventure, ambience, asseteditor, beds, blockphysics, blockspawner, 
blocktick, buildertools, commandmacro, crafting, creativehub, 
crouchslide, deployables, fluid, hytalegenerator, instances, 
landiscovery, mantling, model, mounts, npccombatactionevaluator, 
npceditor, parkour, path, portals, randomtick, safetyroll, sprintforce, 
tagset, teleport, weather, worldgen
```

---

## Datei-Suche

### Klasse finden
```bash
find decompiled -name "KlassenName.java"
```

### In Code suchen
```bash
grep -r "suchbegriff" decompiled/com/hypixel/hytale/
```

### Package-Inhalt
```bash
ls decompiled/com/hypixel/hytale/PACKAGE/
```
