# Vanilla Command Reference

**Status**: Complete (Feb 2026, Update 3)
**Source**: `CommandManager.registerCommands()`, all module `registerCommand()` calls
**Total**: 457 Command-Klassen, ~120 Top-Level-Befehle

---

## 🛡️ Administration

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/op` | `add`, `remove`, `self` | `PermissionsModule` |
| `/kick` | `/kick <player>` | `CommandManager` |
| `/ban` | `/ban <player> [reason] [duration]` | `AccessControlModule` |
| `/unban` | `/unban <player>` | `AccessControlModule` |
| `/stop` | `/stop [crash]` | `CommandManager` |
| `/whitelist` | `add`, `remove`, `list`, `clear`, `enable`, `disable`, `status` | `AccessControlModule` |
| `/perm` | `user`, `group`, `test` | `PermissionsModule` |
| `/sudo` | `/sudo <player/*> <command>` | `CommandManager` |
| `/maxplayers` | `/maxplayers <count>` | `CommandManager` |
| `/who` | Spielerliste anzeigen | `CommandManager` |
| `/auth` | `login` (`browser`, `device`), `logout`, `cancel`, `select`, `status`, `persistence` | `CommandManager` |

## 👤 Player Management

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/gamemode` | `/gamemode <mode> [player]` | `CommandManager` |
| `/give` | `/give <player> <item> [amount] [data]` | `CommandManager` |
| `/kill` | `/kill [player]` | `CommandManager` |
| `/damage` | `/damage <entity> <amount>` | `CommandManager` |
| `/inventory` | `clear`, `see` | `CommandManager` |
| `/player` | `effect` (`apply`, `clear`), `stats` (`get`, `set`, `add`, `reset`, `settomax`, `dump`), `camera`, `respawn` | `CommandManager` |
| `/hide` | Toggle Vanish | `CommandManager` |
| `/whereami` | Position anzeigen | `CommandManager` |
| `/whoami` | Spielerinfo anzeigen | `CommandManager` |
| `/refer` | Referral-System | `CommandManager` |
| `/emote` | `/emote <name>` | `CosmeticsModule` |
| `/sleep` | `offset`, `test` | `CommandManager` |
| `/stash` | Stash/Aufbewahrung | `CommandManager` |
| `/recipe` | `learn`, `forget`, `list` | `CraftingPlugin` |

## 🚀 Teleportation & Warps

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/tp` | `<target>`, `<player> <dest>`, `<x y z>` (Varianten: toPlayer, toCoordinates, otherToPlayer, playerToCoordinates) | `TeleportPlugin` |
| `/back` | Letzte Position | `TeleportPlugin` |
| `/forward` | Nächste Position (nach /back) | `TeleportPlugin` |
| `/top` | Höchster Block | `TeleportPlugin` |
| `/home` | Home-Teleport | `TeleportPlugin` |
| `/warp` | `go`, `set`, `delete`, `list` | `TeleportPlugin` |
| `/spawn` | `set`, `setdefault` | `TeleportPlugin` |
| `/tpworld` | `/tpworld <worldname>` | `TeleportPlugin` |
| `/tphistory` | Teleport-Verlauf | `TeleportPlugin` |

## 🏗️ BuilderTools

Registriert von `BuilderToolsPlugin` — 38 Befehle:

### Selektion

| Command | Beschreibung |
| :--- | :--- |
| `/pos1` | Selektion Ecke 1 |
| `/pos2` | Selektion Ecke 2 |
| `/deselect` | Selektion aufheben |
| `/expand` | Selektion erweitern |
| `/shift` | Selektion verschieben |
| `/contractSelection` | Selektion verkleinern |
| `/selectchunk` | Ganzen Chunk selektieren |
| `/selectchunksection` | Chunk-Sektion selektieren |
| `/selectionHistory` | Selektions-Verlauf |
| `/updateselection` | Selektion aktualisieren |

### Clipboard

| Command | Beschreibung |
| :--- | :--- |
| `/copy` | Kopieren |
| `/cut` | Ausschneiden |
| `/paste` | Einfügen |
| `/flip` | Clipboard spiegeln |
| `/rotate` | Clipboard rotieren |
| `/stack` | Selektion stapeln |
| `/move` | Selektion verschieben |

### Blöcke

| Command | Beschreibung |
| :--- | :--- |
| `/set` | Blöcke setzen |
| `/replace` | Blöcke ersetzen |
| `/fillBlocks` | Selektion füllen |
| `/walls` | Wände erstellen |
| `/hollow` | Selektion aushöhlen |
| `/layer` | Schichtung anwenden (Update 3) |
| `/submerge` | Unter Wasser setzen |
| `/clearBlocks` | Blöcke löschen |
| `/clearEntities` | Entities in Selektion entfernen |
| `/tint` | Block-Tinting |
| `/repairfillers` | Filler-Blöcke reparieren |
| `/gmask` | Globale Maske setzen |
| `/editline` | Linien-Bearbeitung |
| `/extendface` | Fläche erweitern |
| `/environment` | Umgebungs-Einstellungen |

### History

| Command | Beschreibung |
| :--- | :--- |
| `/undo` | Rückgängig |
| `/redo` | Wiederholen |
| `/setToolHistorySize` | History-Größe setzen |

### Import/Export

| Command | Beschreibung |
| :--- | :--- |
| `/prefab` | `load`, `save` |
| `/importobj` | OBJ-Import |
| `/importimage` | Bild-Import |
| `/hotbar` | Hotbar-Wechsel |

### Prefab Editor (`/editprefab`)

| Subcommand | Beschreibung |
| :--- | :--- |
| `new` | Neuen Prefab erstellen |
| `load` | Prefab laden |
| `save` | Prefab speichern |
| `saveas` | Prefab speichern unter |
| `saveui` | Save-UI öffnen |
| `exit` | Editor verlassen |
| `info` | Prefab-Info |
| `modified` | Änderungen anzeigen |
| `select` | Prefab-Box selektieren |
| `killentities` | Entities im Prefab entfernen |
| `setBox` | Prefab-Box setzen |
| `tp` | Zum Prefab teleportieren |

### Scripted Brushes (`/scriptedbrushes`)

| Subcommand | Beschreibung |
| :--- | :--- |
| `load` | Brush laden |
| `list` | Brushes auflisten |
| `clear` | Brush entfernen |
| `exit` | Brush-Editor verlassen |
| `debugstep` | Debug-Schritt |

## 🎥 Camera & Cinematic

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/cameraeffect` | `camshake`, `dilation`, etc. | `CameraPlugin` |

## 📦 Entity & Block

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/entity` | `count`, `remove`, `clean`, `clone`, `dump`, `resend`, `effect`, `stats` (`get`, `set`, `add`, `reset`, `settomax`, `dump`), `snapshot` (`history`, `length`), `nameplate`, `interactable`, `invulnerable`, `intangible`, `lod`, `hidefromadventureplayers`, `tracker` | `CommandManager` |
| `/spawnblock` | `/spawnblock <id> <x> <y> <z>` | `CommandManager` |
| `/spawnitem` | Item-Entity spawnen | `ItemModule` |
| `/blockspawner` | `get`, `set` | `BlockSpawnerPlugin` |
| `/prefabspawner` | `get`, `set`, `weight`, `target` | `PrefabSpawnerModule` |
| `/block` | `get`, `set`, `getstate`, `setstate`, `row`, `inspectfiller`, `inspectphys`, `inspectrotation`, `setticking`, `bulk` (`find`, `find-here`, `replace`) | `Universe` |
| `/blockselect` | Block-Selektion | `Universe` |
| `/blockset` | Block-Set-Management | `BlockSetModule` |

## 🌍 World & Environment

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/time` | `set`, `add`, `pause` | `TimeModule` |
| `/world` | `add`, `list`, `load`, `remove`, `save`, `setdefault`, `prune`, `rocksdb`, `perf` (`graph`, `reset`), `tps` (`reset`) | `Universe` |
| `/worldconfig` | `seed`, `pausetime`, `pvp`, `setspawn`, `setspawndefault`, `pause` | `Universe` |
| `/worldsettings` | Welt-Settings | `Universe` |
| `/worldgen` | `reload`, `benchmark` | `CommandManager` |
| `/worldmap` | `discover`, `undiscover`, `clearmarkers`, `viewradius` | `CommandManager` |
| `/lighting` | `calculation`, etc. | `CommandManager` |
| `/setticking` | Ticking an/aus | `Universe` |
| `/chunk` | `load`, `unload`, `info`, `loaded`, `resend`, `forcetick`, `regenerate`, `fixheight`, `maxsendrate`, `marksave`, `tracker`, `tint`, `lighting` | `CommandManager` |
| `/fluid` | Fluid-System | `FluidPlugin` |
| `/weather` | `/weather <type> [duration]` | (Modul) |

## 🏹 Adventure & Objectives

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/objective` | `start`, `complete`, `panel`, `history`, `locationmarker`, `reachlocationmarker` | `ObjectivePlugin` |
| `/memories` | `unlock`, `unlockAll`, `clear`, `capacity`, `level`, `setCount` | `MemoriesPlugin` |
| `/reputation` | `set`, `add`, `rank`, `value` | `ReputationPlugin` |

## 🤖 NPC & AI

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/npc` | `spawn`, `clean`, `freeze`, `thaw`, `all`, `appearance`, `attack`, `blackboard`, `debug`, `dump`, `give`, `message`, `path`, `role`, `step`, `sensorstats`, `benchmark`, `runtests`, `test` | `NPCPlugin` |
| `/flock` | NPC-Flocking | `server.flock` |

## 🐴 Mounts

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/mount` | `check`, etc. | `MountPlugin` |
| `/dismount` | Absteigen | `MountPlugin` |

## 🏠 Spawning & Instances

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/spawning` | `beacons`, `markers`, `populate`, `stats`, `suppression` | `SpawningPlugin` |
| `/instances` | `spawn`, `exit`, `edit` (`copy`, `list`, `load`, `new`), `migrate` | `InstancesPlugin` |

## 🛤️ Paths & Parkour

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/prefabpath` | `add`, `edit`, `list`, `merge`, `new`, `nodes`, `update`, `observationAngle`, `pause` | `PathPlugin` |
| `/worldpath` | `list`, `remove`, `save`, `builder` | `PathPlugin` |
| `/parkour` | `checkpoint` (`add`, `remove`, `reset`) | `ParkourPlugin` |

## 🌐 Portals & Creative Hub

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/leave` | Portal/Instanz verlassen | `PortalsPlugin` |
| `/cursethis` | Cursed Held Item | `PortalsPlugin` |
| `/voidevent` | Void Event starten | `PortalsPlugin` |
| `/fragment` | Timer Fragment | `PortalsPlugin` |
| `/hub` | Creative Hub | `CreativeHubPlugin` |

## 📡 Networking & Server

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/network` | Network-Debugging | `CommandManager` |
| `/packetstats` | Paket-Statistiken | `CommandManager` |
| `/networkChunkSending` | Chunk-Sending an/aus | `CommandManager` |
| `/ping` | Latenz messen | `CommandManager` |
| `/latencysimulation` | Latenz simulieren | `CommandManager` |
| `/server` | Server-Status/Info | `CommandManager` |

## 🔄 Update System

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/update` | `check`, `download`, `apply`, `cancel`, `status`, `patchline` | `UpdateModule` |

## 🐞 Debug & Dev Tools

| Command | Beschreibung | Source |
| :--- | :--- | :--- |
| `/debug` | `shape` (`arrow`, `cone`, `cube`, `cylinder`, `sphere`, `clear`, `showforce`) | `DebugPlugin` |
| `/hitbox` | Hitbox-Anzeige | `CollisionModule` |
| `/hitboxcollision` | Hitbox-Kollisions-Debug | `CommandManager` |
| `/hitdetection` | Hit-Detection-Debug | `CommandManager` |
| `/debugplayerposition` | Spieler-Position debuggen | `CommandManager` |
| `/repulsion` | Repulsion-Debug | `CommandManager` |
| `/stresstest` | Stresstest starten | `CommandManager` |
| `/tagpattern` | Tag-Pattern-Debug | `CommandManager` |
| `/messagetest` | Message-Translation-Test | `CommandManager` |
| `/hudtest` | HUD-Manager-Test | `CommandManager` |
| `/ui-gallery` | UI-Gallery öffnen | `CommandManager` |
| `/desyncdamage` | Desync-Damage-Debug | `DamageModule` |
| `/interaction` | `run`, `clear`, `snapshotsource`, `specific` | `InteractionModule` |
| `/model` | Model-Override | `ModelPlugin` |
| `/commands` | Command-Dump/Info | `CommandManager` |
| `/validatecpb` | CPB-Validierung | `CommandManager` |
| `/pidcheck` | PID-Check | `CommandManager` |

## 📋 Utility & Misc

| Command | Beschreibung | Source |
| :--- | :--- | :--- |
| `/help` | Hilfe anzeigen | `CommandManager` |
| `/version` | Server-Version | `CommandManager` |
| `/say` | Chat-Broadcast | `ConsoleModule` |
| `/notify` | Benachrichtigung senden | `CommandManager` |
| `/eventtitle` | Event-Titel anzeigen | `CommandManager` |
| `/backup` | Backup erstellen | `CommandManager` |
| `/convertprefabs` | Prefabs konvertieren | `CommandManager` |
| `/particle` | Partikel-Effekte | `CommandManager` |
| `/sound` | Sound abspielen | `CommandManager` |
| `/git` | Git-Integration | `CommandManager` |
| `/log` | Log-Level setzen | `CommandManager` |
| `/bindings` | Key-Bindings | `ServerManager` |
| `/wait` | Makro: Warten | `MacroCommandPlugin` |
| `/echo` | Makro: Echo | `MacroCommandPlugin` |
| `/toggleBlockPlacementOverride` | Block-Placement-Override | `CommandManager` |
| `/showBuilderToolsHud` | BuilderTools-HUD | `CommandManager` |
| `/itemstate` | Item-State anzeigen | `CommandManager` |
| `/droplist` | Droplisten anzeigen | `AssetModule` |
| `/toggletmptags` | Temporäre Tags | `I18nModule` |
| `/landiscovery` | LAN-Discovery | `LANDiscoveryPlugin` |

## 🎮 Singleplayer

| Command | Subcommands / Usage | Source |
| :--- | :--- | :--- |
| `/play` | `friend`, `lan`, `online` | `SingleplayerModule` |

## 🗺️ Assets & Packs

| Command | Beschreibung | Source |
| :--- | :--- | :--- |
| `/assets` | `tags`, `duplicates`, `longest` | `CommandManager` |
| `/packs` | Pack-Management | `CommandManager` |
| `/ambience` | `setmusic`, `emitter` (`add`), `clear` | `AmbiencePlugin` |

## 🏔️ World Generation

| Command | Beschreibung | Source |
| :--- | :--- | :--- |
| `/worldgen` | `reload`, `benchmark` | `CommandManager` |
| `/viewport` | Viewport für Generator | `HytaleGenerator` |

## 🔧 Plugin Management

| Command | Beschreibung | Source |
| :--- | :--- | :--- |
| `/plugin` | Plugin-Verwaltung | `PluginCommand` |

## ⏱️ Internationalization

| Command | Beschreibung | Source |
| :--- | :--- | :--- |
| `/lang` | Sprach-Befehle | `I18nModule` |
| `/toggletmptags` | Temp-Tags aktivieren | `I18nModule` |
