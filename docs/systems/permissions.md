# Permissions System

> **Status**: Verified against Decompiled Server Code (February 2026).
> **Source**: `com.hypixel.hytale.server.core.permissions.HytalePermissions`, `PermissionProvider`

The Hytale Permission System allows controlling access to commands and features for players and groups.

## 1. Checking Permissions

To check if a command sender (Player or Console) has a permission:

```java
public void execute(CommandContext ctx) {
    if (ctx.sender().hasPermission("my.custom.node")) {
        // Access granted
    } else {
        ctx.sender().sendMessage(Message.text("No permission!"));
    }
}
```

## 2. Standard Permission Nodes

The following nodes are defined in `HytalePermissions`:

| Permission Node | Description |
|-----------------|-------------|
| `hytale.command.*` | Base for all commands |
| `hytale.camera.flycam` | Access to the Fly Cam |
| `hytale.editor.asset` | Asset Editor access |
| `hytale.editor.builderTools` | Builder Tools access |
| `hytale.world_map.teleport.coordinate` | Teleport via Map Coords |
| `hytale.world_map.teleport.marker` | Teleport via Map Markers |
| `hytale.system.update.notify` | Receive update notifications |

### Asset Editor Permissions
*   `hytale.editor.packs.create`
*   `hytale.editor.packs.edit`
*   `hytale.editor.packs.delete`
*   `hytale.editor.brush.use`
*   `hytale.editor.brush.config`
*   `hytale.editor.prefab.use`
*   `hytale.editor.prefab.manage`

## 3. Managing Permissions (`PermissionProvider`)

You can modify permissions at runtime using the `PermissionProvider`.

```java
import com.hypixel.hytale.server.core.permissions.provider.PermissionProvider;

// Get the provider
PermissionProvider provider = HytaleServer.get().getPermissionProvider();

// Add Permission to User
provider.addUserPermissions(playerUUID, Set.of("my.custom.node"));

// Remove Permission
provider.removeUserPermissions(playerUUID, Set.of("my.custom.node"));

// Group Management
provider.addGroupPermissions("admins", Set.of("hytale.command.*"));
provider.addUserToGroup(playerUUID, "admins");
```

## 4. Manifest Configuration

Permissions are **NOT** defined in `manifest.json`. They are handled dynamically by plugins or the server configuration.

---

## 5. Permission Events (Update 3)

> **Update 3**: Permission-Events haben ein eigenes Subpackage erhalten: `com.hypixel.hytale.server.core.event.events.permissions`

| Event | Beschreibung |
|-------|-------------|
| `PlayerPermissionChangeEvent` | Einzelne Permission eines Spielers geändert |
| `PlayerGroupEvent` | Spieler-Gruppe geändert |
| `GroupPermissionChangeEvent` | Permission einer Gruppe geändert (Update 3) |

```java
getEventRegistry().register(GroupPermissionChangeEvent.class, event -> {
    getLogger().info("Group permission changed: " + event.getGroup());
});
```
