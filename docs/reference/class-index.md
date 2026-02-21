# Class Index

> **LLM Quick Lookup**: Klasse → Pfad → Zweck

---

## Core Modding API

| Klasse | Pfad | Beschreibung |
|--------|------|--------------|
| `JavaPlugin` | `server/core/plugin/JavaPlugin.java` | Main Plugin Class |
| `PluginBase` | `server/core/plugin/PluginBase.java` | Plugin API Base |
| `ClassTransformer` | `plugin/early/ClassTransformer.java` | Mod-Einstiegspunkt |
| `EarlyPluginLoader` | `plugin/early/EarlyPluginLoader.java` | JAR-Loader |
| `TransformingClassLoader` | `plugin/early/TransformingClassLoader.java` | ClassLoader |

---

## Event System

| Klasse | Pfad | Beschreibung |
|--------|------|--------------|
| `IEvent` | `event/IEvent.java` | Sync Event Interface |
| `IAsyncEvent` | `event/IAsyncEvent.java` | Async Event Interface |
| `IBaseEvent` | `event/IBaseEvent.java` | Basis Event Interface |
| `ICancellable` | `event/ICancellable.java` | Cancellable Mixin |
| `IEventBus` | `event/IEventBus.java` | Event Dispatcher |
| `IEventRegistry` | `event/IEventRegistry.java` | Event Registration (24 Methoden) |
| `EventPriority` | `event/EventPriority.java` | Enum: FIRST/EARLY/NORMAL/LATE/LAST |
| `EventRegistration` | `event/EventRegistration.java` | Registration Handle |

---

## ECS System (component/)

| Klasse | Größe | Pfad | Beschreibung |
|--------|-------|------|--------------|
| `Store` | 98KB | `component/Store.java` | Haupt-Entity-Speicher |
| `ComponentRegistry` | 72KB | `component/ComponentRegistry.java` | Component-Type-Registry |
| `Holder` | 23KB | `component/Holder.java` | Entity-Container |
| `Archetype` | 12KB | `component/Archetype.java` | Component-Set Definition |
| `ArchetypeChunk` | 13KB | `component/ArchetypeChunk.java` | Archetype-Chunks |
| `Component` | 0.5KB | `component/Component.java` | Component Interface |
| `ComponentType` | - | `component/ComponentType.java` | Type Marker |

### Sub-Packages

| Package | Pfad | Inhalt |
|---------|------|--------|
| `data/` | `component/data/` | Datenstrukturen |
| `query/` | `component/query/` | Entity Queries |
| `system/` | `component/system/` | ECS Systems |
| `event/` | `component/event/` | ECS Events |
| `spatial/` | `component/spatial/` | Räumliche Indexierung |

---

## Protocol (Größte Dateien)

| Klasse | Größe | Pfad | Beschreibung |
|--------|-------|------|--------------|
| `BlockType` | 102KB | `protocol/BlockType.java` | Block-Definitionen |
| `Weather` | 101KB | `protocol/Weather.java` | Wetter-System |
| `ItemBase` | 100KB | `protocol/ItemBase.java` | Item-Definitionen |
| `PacketRegistry` | 85KB | `protocol/PacketRegistry.java` | Packet-Registry |
| `PlayerSkin` | 55KB | `protocol/PlayerSkin.java` | Skin-System |
| `ComponentUpdate` | 54KB | `protocol/ComponentUpdate.java` | ECS-Sync |
| `Model` | 49KB | `protocol/Model.java` | Modell-Daten |
| `ItemArmor` | 36KB | `protocol/ItemArmor.java` | Rüstungen |
| `DamageEntityInteraction` | 37KB | `protocol/DamageEntityInteraction.java` | Schaden |
| `WieldingInteraction` | 34KB | `protocol/WieldingInteraction.java` | Wielding |
| `MovementSettings` | 32KB | `protocol/MovementSettings.java` | Bewegung |
| `ChargingInteraction` | 30KB | `protocol/ChargingInteraction.java` | Chargen |
| `FormattedMessage` | 30KB | `protocol/FormattedMessage.java` | Nachrichten |
| `ApplyForceInteraction` | 29KB | `protocol/ApplyForceInteraction.java` | Kräfte |
| `ChainingInteraction` | 30KB | `protocol/ChainingInteraction.java` | Verkettung |

---

## NPC/AI (server/npc/)

| Package | Pfad | Inhalt |
|---------|------|--------|
| `NPCPlugin` | `server/npc/NPCPlugin.java` | 92KB Haupt-Plugin |
| `blackboard/` | `server/npc/blackboard/` | AI-Daten |
| `decisionmaker/` | `server/npc/decisionmaker/` | Entscheidungen |
| `instructions/` | `server/npc/instructions/` | AI-Befehle |
| `role/` | `server/npc/role/` | NPC-Rollen |
| `statetransition/` | `server/npc/statetransition/` | State Machine |
| `movement/` | `server/npc/movement/` | Bewegung |
| `navigation/` | `server/npc/navigation/` | Pathfinding |
| `sensorinfo/` | `server/npc/sensorinfo/` | Sensoren |
| `components/` | `server/npc/components/` | ECS Components |

---

## WorldGen (server/worldgen/)

| Package | Pfad | Inhalt |
|---------|------|--------|
| `biome/` | `server/worldgen/biome/` | Biom-System |
| `climate/` | `server/worldgen/climate/` | Klima |
| `cave/` | `server/worldgen/cave/` | Höhlen |
| `prefab/` | `server/worldgen/prefab/` | Strukturen |
| `zone/` | `server/worldgen/zone/` | Welt-Zonen |
| `chunk/` | `server/worldgen/chunk/` | Chunks |
| `loader/` | `server/worldgen/loader/` | Asset-Loader |

---

## Builtin Modules (32)

| Modul | Pfad | Beschreibung |
|-------|------|--------------|
| `adventure` | `builtin/adventure/` | Quests, Objectives |
| `ambience` | `builtin/ambience/` | Umgebung |
| `asseteditor` | `builtin/asseteditor/` | Asset-Editor |
| `beds` | `builtin/beds/` | Bett/Schlaf-System (Update 3) |
| `blockphysics` | `builtin/blockphysics/` | Block-Physik |
| `blockspawner` | `builtin/blockspawner/` | Block-Spawning |
| `blocktick` | `builtin/blocktick/` | Block-Ticks |
| `buildertools` | `builtin/buildertools/` | Builder-Werkzeuge |
| `commandmacro` | `builtin/commandmacro/` | Command-Makros |
| `crafting` | `builtin/crafting/` | Crafting |
| `creativehub` | `builtin/creativehub/` | Kreativ-Hub ("Crossroads") |
| `crouchslide` | `builtin/crouchslide/` | Rutschen |
| `deployables` | `builtin/deployables/` | Platzierbare Gegenstände |
| `fluid` | `builtin/fluid/` | Flüssigkeiten |
| `hytalegenerator` | `builtin/hytalegenerator/` | Weltgenerierung |
| `instances` | `builtin/instances/` | Dungeons |
| `landiscovery` | `builtin/landiscovery/` | Gebiets-Entdeckung |
| `mantling` | `builtin/mantling/` | Klettern |
| `model` | `builtin/model/` | Modell-System |
| `mounts` | `builtin/mounts/` | Reittiere |
| `npccombatactionevaluator` | `builtin/npccombatactionevaluator/` | NPC-Kampf-AI |
| `npceditor` | `builtin/npceditor/` | NPC-Editor |
| `parkour` | `builtin/parkour/` | Parkour |
| `path` | `builtin/path/` | Pfade/Wege |
| `portals` | `builtin/portals/` | Portale |
| `randomtick` | `builtin/randomtick/` | Random Block Ticking (Update 3) |
| `safetyroll` | `builtin/safetyroll/` | Abroll-Mechanik |
| `sprintforce` | `builtin/sprintforce/` | Sprint-Mechanik |
| `tagset` | `builtin/tagset/` | Tag-Gruppierungen |
| `teleport` | `builtin/teleport/` | Teleportation |
| `weather` | `builtin/weather/` | Wetter |
| `worldgen` | `builtin/worldgen/` | Weltgen-Plugin |

---

## Item Types

| Klasse | Pfad | Größe |
|--------|------|-------|
| `ItemBase` | `protocol/ItemBase.java` | 100KB |
| `ItemWeapon` | `protocol/ItemWeapon.java` | 13KB |
| `ItemArmor` | `protocol/ItemArmor.java` | 36KB |
| `ItemTool` | `protocol/ItemTool.java` | 6KB |
| `ItemUtility` | `protocol/ItemUtility.java` | 13KB |
| `ItemGlider` | `protocol/ItemGlider.java` | 3KB |
| `ItemCategory` | `protocol/ItemCategory.java` | 15KB |
| `ItemQuality` | `protocol/ItemQuality.java` | 24KB |

---

## Block Types

| Klasse | Pfad | Größe |
|--------|------|-------|
| `BlockType` | `protocol/BlockType.java` | 102KB |
| `BlockTextures` | `protocol/BlockTextures.java` | 17KB |
| `BlockSoundSet` | `protocol/BlockSoundSet.java` | 10KB |
| `BlockParticleSet` | `protocol/BlockParticleSet.java` | 13KB |
| `BlockBreaking` | `protocol/BlockBreaking.java` | 11KB |
| `BlockGroup` | `protocol/BlockGroup.java` | 6KB |
| `BlockSet` | `protocol/BlockSet.java` | 8KB |
| `Fluid` | `protocol/Fluid.java` | 23KB |

---

## Interactions

| Klasse | Pfad | Größe |
|--------|------|-------|
| `Interaction` | `protocol/Interaction.java` | 26KB |
| `DamageEntityInteraction` | `protocol/DamageEntityInteraction.java` | 37KB |
| `WieldingInteraction` | `protocol/WieldingInteraction.java` | 34KB |
| `ChargingInteraction` | `protocol/ChargingInteraction.java` | 30KB |
| `ChainingInteraction` | `protocol/ChainingInteraction.java` | 30KB |
| `ApplyForceInteraction` | `protocol/ApplyForceInteraction.java` | 29KB |
| `ModifyInventoryInteraction` | `protocol/ModifyInventoryInteraction.java` | 28KB |
| `SelectInteraction` | `protocol/SelectInteraction.java` | 26KB |

---

## Alle Pfade relativ zu

```
decompiled/com/hypixel/hytale/
```

### Beispiel absoluter Pfad:
```
decompiled/com/hypixel/hytale/event/IEventRegistry.java
```
