# Networking Specification

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.protocol.PacketRegistry`.
> **SOURCE**: Decompiled `PacketRegistry.java`.

## 1. Packet Table (ID -> Class)

This table maps every Protocol ID to its Packet Class. Use this for packet interception and construction.

| ID | Name | Class |
|---|---|---|
| 0 | Connect | `com.hypixel.hytale.protocol.packets.connection.Connect` |
| 1 | Disconnect | `com.hypixel.hytale.protocol.packets.connection.Disconnect` |
| 20 | WorldSettings | `com.hypixel.hytale.protocol.packets.setup.WorldSettings` |
| 23 | RequestAssets | `com.hypixel.hytale.protocol.packets.setup.RequestAssets` |
| 24 | AssetInitialize | `com.hypixel.hytale.protocol.packets.setup.AssetInitialize` |
| 40 | UpdateBlockTypes | `com.hypixel.hytale.protocol.packets.assets.UpdateBlockTypes` |
| 54 | UpdateItems | `com.hypixel.hytale.protocol.packets.assets.UpdateItems` |
| 104 | JoinWorld | `com.hypixel.hytale.protocol.packets.player.JoinWorld` |
| 108 | ClientMovement | `com.hypixel.hytale.protocol.packets.player.ClientMovement` |
| 111 | MouseInteraction | `com.hypixel.hytale.protocol.packets.player.MouseInteraction` |
| 117 | ClientPlaceBlock | `com.hypixel.hytale.protocol.packets.player.ClientPlaceBlock` |
| 140 | ServerSetBlock | `com.hypixel.hytale.protocol.packets.world.ServerSetBlock` |
| 152 | SpawnParticleSystem | `com.hypixel.hytale.protocol.packets.world.SpawnParticleSystem` |
| 162 | PlayAnimation | `com.hypixel.hytale.protocol.packets.entities.PlayAnimation` |
| 200 | OpenWindow | `com.hypixel.hytale.protocol.packets.window.OpenWindow` |
| 211 | ChatMessage | `com.hypixel.hytale.protocol.packets.interface_.ChatMessage` |
| 235 | UpdateAnchorUI | `com.hypixel.hytale.protocol.packets.interface_.UpdateAnchorUI` |

*(Note: Full list available in `PacketRegistry.java` dump)*

## 2. Packet Implementation Pattern

To create a **custom packet**, you must implement `Packet`:

```java
import com.hypixel.hytale.protocol.Packet;
import io.netty.buffer.ByteBuf;

public class MyCustomPacket implements Packet {
    public static final int ID = 9001; // Must be unique (collisions likely with mods)
    
    private String message;
    
    public MyCustomPacket(String message) {
        this.message = message;
    }
    
    @Override
    public int getId() {
        return ID;
    }
    
    @Override
    public void serialize(ByteBuf buf) {
        PacketIO.writeVarString(buf, message);
    }
    
    // Constructor for deserialization
    public static MyCustomPacket deserialize(ByteBuf buf) {
        return new MyCustomPacket(PacketIO.readVarString(buf));
    }
    
    @Override
    public int computeSize() {
        return PacketIO.computeVarStringSize(message);
    }
}
```

## 3. Packet Categories (Verified)
 
 Verified standard packet channels (17 categories):
 
 *   `asseteditor` - In-game editor tools.
 *   `assets` - Resource pack syncing.
 *   `auth` - Handshake & Authentication.
 *   `buildertools` - Copy/Paste, Schematics.
 *   `camera` - Client camera control.
 *   `connection` - Ping/Pong, Disconnect.
 *   `entities` - Spawning, Movement, Metadata.
 *   `interaction` - Hits, Clicks.
 *   `interface_` - HUD, Notifications, Chat.
 *   `inventory` - Window items, Slot updates.
 *   `machinima` - Replay recording.
 *   `player` - Skins, GameMode, Stats.
 *   `serveraccess` - Admin tools.
 *   `setup` - Initial loading screen data.
 *   `window` - UI Menus.
 *   `world` - Chunks, Block changes.
 *   `worldmap` - Map markers, Teleportation.
 
 ## 4. Packet Registration

Custom packets are risky as IDs are static.
**Preferred method**: Use `CustomPageEvent` (ID 219) or `OpenChatWithCommand` (ID 234) for data transfer if possible, avoiding raw packet registration unless necessary.

If registration is required, intercept `PacketRegistry` via ClassTransformer (Early Plugin).
