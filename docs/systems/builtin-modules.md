# Built-in Modules

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.server.builtin.*` modules.
> **SOURCE**: Decompiled `HytaleServer.jar`.

## Module List

| Module | Description |
|--------|-------------|
| `adventure` | Quests, Objectives, Treasure Chests |
| `ambience` | Umgebungseffekte |
| `asseteditor` | Asset-Editor |
| `beds` | Bett- und Schlaf-System (Update 3) |
| `blockphysics` | Block-Physik |
| `blockspawner` | Block-Spawning |
| `blocktick` | Block-Updates über Zeit |
| `buildertools` | Builder-Werkzeuge (Kreativ) |
| `commandmacro` | Command-Makros |
| `crafting` | Crafting-Rezepte und Stationen |
| `creativehub` | Kreativ-Hub ("Crossroads" seit Update 3) |
| `crouchslide` | Rutschen-Mechanik |
| `deployables` | Platzierbare Gegenstände |
| `fluid` | Flüssigkeits-Physik |
| `hytalegenerator` | Weltgenerierung |
| `instances` | Instanzen/Dungeons |
| `landiscovery` | Gebiets-Entdeckung |
| `mantling` | Klettern/Hochziehen |
| `model` | Modell-System |
| `mounts` | Reittiere |
| `npccombatactionevaluator` | NPC-Kampf-AI |
| `npceditor` | NPC-Editor |
| `parkour` | Parkour-Mechaniken |
| `path` | Pfade/Wege |
| `portals` | Portal-System |
| `randomtick` | Random Block Ticking — Gras-Ausbreitung etc. (Update 3) |
| `safetyroll` | Abroll-Mechanik |
| `sprintforce` | Sprint-Mechanik |
| `tagset` | Tag-Gruppierungen |
| `teleport` | Teleportation |
| `weather` | Wetter-System |
| `worldgen` | Welt-Generierung |

---

## Adventure-System

Quest und Objectives:

```
builtin/adventure/
├── objectives/
│   ├── events/
│   │   └── TreasureChestOpeningEvent.java
│   └── blockstates/
│       └── TreasureChestState.java
```

### Events
- `TreasureChestOpeningEvent` - Truhe wird geöffnet

---

## Block-Systeme

### BlockPhysics

```java
// builtin/blockphysics/
BlockPhysicsPlugin      // Plugin-Entry
BlockPhysicsUtil        // Hilfsfunktionen
BlockPhysicsSystems     // ECS-Systems
WorldValidationUtil     // Welt-Validierung
PrefabBufferValidator   // Prefab-Validierung
```

### BlockSpawner

```java
// builtin/blockspawner/
BlockSpawnerPlugin      // Plugin
BlockSpawnerEntry       // Spawn-Eintrag
BlockSpawnerTable       // Spawn-Tabelle
BlockSpawner           // State-Komponente

// Commands
BlockSpawnerCommand
BlockSpawnerSetCommand
BlockSpawnerGetCommand
```

### BlockTick

Block-Updates über Zeit (Wachstum, Verfall):

```java
// builtin/blocktick/
BlockTickPlugin
ChunkBlockTickSystem    // Chunk-weise Ticks
MergeWaitingBlocksSystem

// Procedures
BasicChanceBlockGrowthProcedure
SplitChanceBlockGrowthProcedure
```

---

## Crafting-System

```
builtin/crafting/
├── recipes/           # Rezept-Definitionen
├── stations/          # Crafting-Stationen
└── components/        # ECS-Komponenten
```

---

## Instances (Dungeons)

```java
// builtin/instances/
DiscoverInstanceEvent   // Instanz entdeckt

// Blocks
InstanceBlock
ConfigurableInstanceBlock
```

---

## Portals

Portal- und Dimensions-System:

```java
// builtin/portals/
// Void-Events für spezielle Portal-Mechaniken
VoidEvent
VoidEventConfig
VoidEventStage
VoidEventCommands
StartVoidEventCommand
```

---

## Mounts

Reittier-System:

```
builtin/mounts/
├── components/       # Mount-Komponenten
├── systems/          # Mount-ECS-Systems
└── config/           # Mount-Konfiguration
```

---

## Weather

Wetter-System:

```
builtin/weather/
├── WeatherPlugin
├── types/            # Wettertypen
└── effects/          # Wetter-Effekte
```

---

## Worldgen

Welt-Generierung (zusätzlich zu `server/worldgen`):

```
builtin/worldgen/
└── HytaleGenerator/
    └── assets/
        ├── blockset/    # Material-Sets
        └── blockmask/   # Block-Masken
```

---

## Plugin-Pattern

Jedes Modul folgt dem Plugin-Pattern:

```java
public class MyModule extends JavaPlugin {

    public MyModule(JavaPluginInit init) {
        super(init);
    }

    @Override
    public void start() {
        getLogger().info("Hello");
        // Components registrieren
        // Systems registrieren
        // Events registrieren
        // Commands registrieren
    }
}
```

---

## Modding-Ansätze

1. **Bestehende Module erweitern**: Events abonnieren
2. **Neue Module erstellen**: Plugin-Pattern folgen
3. **Verhalten überschreiben**: ClassTransformer nutzen
4. **Konfiguration ändern**: Asset-Dateien modifizieren

---

## Weiterführend

- [NPC/AI-System](npc-ai.md)
- [Event-System](events.md)
- [ECS-System](../architecture/ecs.md)
