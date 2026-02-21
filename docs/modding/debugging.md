# Debugging & Developer Tools

> **CONTEXT**: Reference for server launch arguments and debug commands.
> **SOURCE**: `com.hypixel.hytale.server.core.Options`, `com.hypixel.hytale.server.core.modules.debug.commands.DebugCommand`.

## 1. Launch Arguments (CLI)

Use these arguments when starting the server jar to enable dev modes or unsafe features.

| Argument | Description |
| :--- | :--- |
| `--help` | Prints help message |
| `--version` | Prints Hytale Server version |
| `--bare` | Runs server without loading worlds/binding ports |
| `--log LEVEL` | Sets logger level (e.g., `active:DEBUG`) |
| `--bind HOST:PORT` | Port to listen on (Default: 5520) |
| `--transport TYPE` | Transport type (Default: QUIC) |
| `--disable-cpb-build` | Disables Compact Prefab Buffer build |
| `--prefab-cache PATH` | Prefab cache directory |
| `--assets PATH` | Asset directory (Default: `../HytaleAssets`) |
| `--mods PATH` | Additional mods directories |
| `--accept-early-plugins` | Acknowledge Early Plugin risks |
| `--early-plugins PATH` | Additional early plugin directories |
| `--validate-assets` | Exit if assets are invalid |
| `--validate-prefabs` | Exit if prefabs are invalid |
| `--validate-world-gen` | Exit if world gen is invalid |
| `--shutdown-after-validate`| Shutdown after validation checks |
| `--generate-schema` | Generate schema and exit |
| `--world-gen PATH` | World gen directory |
| `--disable-file-watcher` | Disable hot-reloading (Required for debugging) |
| `--disable-sentry` | Disable Sentry error reporting |
| `--disable-asset-compare` | Disable asset comparison |
| `--backup` | Enable backups |
| `--backup-frequency MIN` | Backup frequency (Default: 30) |
| `--backup-dir PATH` | Backup directory |
| `--backup-max-count N` | Max backups to keep (Default: 5) |
| `--backup-archive-max-count N` | Max archived backups (Default: 5, Update 3) |
| `--singleplayer` | Enable singleplayer mode internal checks |
| `--owner-name NAME` | Server owner name |
| `--owner-uuid UUID` | Server owner UUID |
| `--client-pid PID` | Client process ID (for auto-shutdown) |
| `--universe PATH` | Universe directory |
| `--event-debug` | Verbose event logging |
| `--force-network-flush` | Force network flush (Default: true) |
| `--boot-command CMD` | Run command on boot |
| `--allow-op` | Allow self-op command |
| `--auth-mode MODE` | Auth mode (`AUTHENTICATED`, `OFFLINE`, `INSECURE`) |
| `--session-token TOKEN` | Session token |
| `--identity-token TOKEN` | Identity token (JWT) |
### Example Dev Launch Script
```bash
java -jar HytaleServer.jar --singleplayer --accept-early-plugins --disable-file-watcher
```

## 2. In-Game Debug Commands

The `/debug` command contains subcommands for visualizing server state.

*   `/debug shapes`: Controls the rendering of debug shapes (hitboxes, pathfinding nodes) on the client.
    *   Requires the client to be in a dev/debug mode to actually render them.

### Update 3: Neue Debug-/Admin-Befehle

| Command | Beschreibung |
|---------|-------------|
| `/layer` | Area-Layer-Management (Gebiets-Schichtung) |
| `/worldmap` | World-Map-Verwaltung (Marker, Regionen) |
| `/validate` | Asset-/Prefab-Validierung |
| `/version` | Server-Version anzeigen |
| `/bot` | Bot-Management |
| `/damage` | Entity-Schaden testen |

> **Vollständige Liste**: Siehe [Vanilla Command Reference](../reference/vanilla-commands.md)

## 3. Java Debugging (JDWP)

To attach a debugger (IntelliJ/Eclipse) to the running server:

1.  Add JVM Argument:
    `-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005`
2.  Add Game Argument:
    - `--disable-file-watcher` disables hot-reload (prevents the server from killing itself when you pause at a breakpoint).
3.  Connect via Remote JVM Debug on port 5005.

## 4. Common Exceptions

*   `MissingPluginDependencyException`: You are missing a required dependency defined in `manifest.json`.
*   `ConfusedCFRException`: (Internal) The decompiler or ASM transformer failed to read a class. Check your Early Plugins.
*   `SerializationException`: Your Packet/Asset JSON structure doesn't match the expected schema.
