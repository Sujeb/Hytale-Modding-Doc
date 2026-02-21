# Hytale Modding - Getting Started

This guide explains how to create Hytale Mods.

> **NOTE**: There are two types of mods:
> 1.  **Game Plugins (Standard)**: For adding items, blocks, entities, and gameplay logic. **Start here.**
> 2.  **Early Plugins (Advanced)**: For low-level bytecode manipulation.

---

## 1. Prerequisites
- **Java SDK 21+** (Required, Hyxin uses Java 25)
- Build Tool (Maven/Gradle)
- IDE (IntelliJ IDEA recommended)

---

## 2. Standard Game Mod (Recommended)

### A. Project Structure
```
my-mod/
├── pom.xml
├── src/main/java/com/example/mymod/MyMod.java
└── src/main/resources/manifest.json
```

### B. `manifest.json`
Required at the root of your resources.
```json
{
  "Name": "MyMod",
  "Group": "com.example",
  "Version": "1.0.0",
  "Main": "com.example.mymod.MyMod",
  "ServerVersion": "1.0.0",
  "Dependencies": {}
}
```

### C. Main Class (`JavaPlugin`)
```java
package com.example.mymod;

import com.hypixel.hytale.server.core.plugin.JavaPlugin;
import com.hypixel.hytale.server.core.plugin.JavaPluginInit;
import com.hypixel.hytale.server.core.event.events.player.PlayerConnectEvent;
import com.hypixel.hytale.logger.HytaleLogger;

public class MyMod extends JavaPlugin {
    
    public MyMod(JavaPluginInit init) {
        super(init);
    }
    
    // Update 3: Optional preLoad (vor setup), gibt CompletableFuture zurück
    @Override
    public CompletableFuture<Void> preLoad() {
        this.getLogger().info("MyMod pre-loading...");
        return CompletableFuture.completedFuture(null);
    }
    
    @Override
    public void setup() {
        HytaleLogger logger = this.getLogger();
        logger.info("MyMod is setting up!");
    }
    
    @Override
    public void start() {
        // Register Event Listeners in start(), not setup()
        this.getEventRegistry().register(PlayerConnectEvent.class, event -> {
             this.getLogger().info("Player joined: " + event.getPlayerRef().toString());
        });
    }
    
    @Override
    public void shutdown() {
        this.getLogger().info("MyMod shutting down, cleaning up...");
    }
}
```

---

## 3. Advanced: Early Plugin (Class Transformer)

Use this **ONLY** if you need to modify game code directly (ASM/Bytecode).

### A. Structure
```
src/main/resources/META-INF/services/com.hypixel.hytale.plugin.early.ClassTransformer
```

### B. Implementation
```java
public class MyTransformer implements ClassTransformer {
    @Override
    public byte[] transform(String name, String transformedName, byte[] bytes) {
        // ASM Logic
        // Return null to indicate no changes
        return null;
    }
}
```
