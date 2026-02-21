# Early Plugin System Specification

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.plugin.early`.
> **SOURCE**: Decompiled `EarlyPluginLoader` & `TransformingClassLoader`.
> **WARNING**: This is an internal/advanced API. Incorrect usage will crash the game.

## 1. System Overview

Early Plugins run **before** the main game class is loaded, allowing for raw bytecode manipulation of the engine itself.
They are loaded from the `earlyplugins/` directory (or via command line args).

> **CONCEPT: Mixins vs ClassTransformers**
> In modding (e.g., Minecraft/Fabric), "Mixin" usually refers to a specific library (SpongePowered Mixin) that uses annotations like `@Mixin`.
> **Hytale does NOT use this library.** instead, it provides a raw **`ClassTransformer`** interface exposed via Java's `ServiceLoader`.
> You must write raw ASM (Objectweb ASM) code to manipulate bytecode. The *concept* is the same (modifying code at runtime), but the *implementation* is lower-level.

**Core Interface**: `com.hypixel.hytale.plugin.early.ClassTransformer`

## 2. Implementation Pattern

### Step 1: Implement ClassTransformer
```java
package com.example.early;

import com.hypixel.hytale.plugin.early.ClassTransformer;
import org.objectweb.asm.ClassReader;
import org.objectweb.asm.ClassWriter;
import org.objectweb.asm.tree.ClassNode;
// ... standard ASM imports

public class MyTransformer implements ClassTransformer {
    
    @Override
    public int priority() {
        return 100; // Higher runs earlier
    }

    @Override
    public byte[] transform(String name, String transformedName, byte[] bytes) {
        // Target specific classes
        if ("com.hypixel.hytale.server.core.Options".equals(name)) {
            return transformOptions(bytes);
        }
        // Return null to indicate no changes (Optimization)
    return null; // Return unmodified if not target
    }

    private byte[] transformOptions(byte[] basicClass) {
        // Standard ASM transformation
        ClassReader reader = new ClassReader(basicClass);
        ClassNode node = new ClassNode();
        reader.accept(node, 0);
        
        // ... manipulate functions/fields ...
        
        ClassWriter writer = new ClassWriter(ClassWriter.COMPUTE_MAXS | ClassWriter.COMPUTE_FRAMES);
        node.accept(writer);
        return writer.toByteArray();
    }
}
```

### Step 2: Register via ServiceLoader
Create file: `src/main/resources/META-INF/services/com.hypixel.hytale.plugin.early.ClassTransformer`
Content:
```
com.example.early.MyTransformer
```

## 3. Deployment
*   **Compile** to a JAR.
*   **Place** in `earlyplugins/` directory (create if missing).
*   **Note**: Early plugins do NOT use `manifest.json`.

## 4. Restrictions & Environment
*   **Secure Packages**: You CANNOT transform classes in these packages:
    *   `java.*`, `javax.*`, `sun.*`
    *   `org.objectweb.asm.*`, `org.slf4j.*`
    *   `com.hypixel.hytale.plugin.early.*`
*   **Available Libraries**: ASM is available on the classpath (protected package `org.objectweb.asm`).
