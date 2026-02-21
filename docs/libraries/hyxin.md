# Hyxin Library (Mixin for Hytale)

> **CONTEXT**: Documentation for the "Hyxin" community library.
> **SOURCE**: `mods/hyxin` directory in workspace (Community Code).
> **GOAL**: Enable safe, structured bytecode manipulation using the Sponge Mixin framework.

## 1. Overview

**Hyxin** is a library that wraps Hytale's raw `ClassTransformer` system to provide the **Sponge Mixin** framework.
This allows you to use annotations like `@Mixin`, `@Inject`, and `@Redirect` instead of writing raw ASM.

> **RECOMMENDATION**: Always use Hyxin over raw `ClassTransformer` if you need to modify game code.

## 2. Installation

Hyxin must be included in your project (likely shadowed) or installed as a separate Early Plugin.

### Gradle Dependencies
```groovy
repositories {
    mavenCentral()
    maven { url "https://repo.spongepowered.org/repository/maven-public/" }
}

dependencies {
    // Core Mixin Library
    implementation "net.fabricmc:sponge-mixin:0.12.5+mixin.0.8.5"
    // Hyxin (Assumed local or submodule)
    implementation project(":hyxin")
}
```

## 3. Configuration (`hyxin.json`)

To use Mixins, your plugin must provide a Mixin configuration file required by Hyxin.

**File**: `src/main/resources/mixins.mymanifest.json`
```json
{
  "required": true,
  "minVersion": "0.8",
  "package": "com.example.myplugin.mixin",
  "compatibilityLevel": "JAVA_21",
  "Configs": [
    "ExampleMixin"
  ],
  "injectors": {
    "defaultRequire": 1
  }
}
```

## 4. Usage Pattern

### A. The Mixin Class
```java
package com.example.myplugin.mixin;

import com.hypixel.hytale.server.core.HytaleServer;
import org.spongepowered.asm.mixin.Mixin;
import org.spongepowered.asm.mixin.injection.At;
import org.spongepowered.asm.mixin.injection.Inject;
import org.spongepowered.asm.mixin.injection.callback.CallbackInfo;

@Mixin(HytaleServer.class)
public class ServerMixin {

    @Inject(method = "start", at = @At("HEAD"))
    private void onStart(CallbackInfo ci) {
        System.out.println("Mixin injected into Server Start!");
    }
}
```

### B. Standard Plugin Manifest
You may need to declare your Mixin config in your `manifest.json` if Hyxin supports auto-discovery (Custom feature of Hyxin), or register it manually.

*Note: Hyxin scans `earlyplugins` for Mixin configurations.*

## 5. Advantages over Raw ClassTransformer
1.  **Readability**: Annotations are easier to read than ASM opcodes.
2.  **Safety**: Mixin handles stack frames and class verification.
3.  **Conflict Resolution**: Multiple Mods can mixin to the same method (usually) without crashing, whereas raw Transformers often overwrite each other.
