# Common Modding Patterns

> **LLM-optimiert**: Copy-paste-ready Code-Snippets

---

## Mod erstellen

### 1. Transformer-Klasse

```java
package com.meinmod;

import com.hypixel.hytale.plugin.early.ClassTransformer;
import javax.annotation.Nonnull;
import javax.annotation.Nullable;

public class MeinTransformer implements ClassTransformer {
    
    @Override
    public int priority() {
        return 100;  // höher = früher
    }
    
    @Override
    @Nullable
    public byte[] transform(@Nonnull String className, 
                           @Nonnull String transformedName, 
                           @Nonnull byte[] classBytes) {
        // className: "com/hypixel/hytale/server/X" (mit Slashes)
        // Return null = keine Änderung
        // Return byte[] = modifizierter Bytecode
        return null;
    }
}
```

### 2. ServiceLoader-Datei

Erstelle: `src/main/resources/META-INF/services/com.hypixel.hytale.plugin.early.ClassTransformer`

```
com.meinmod.MeinTransformer
```

### 3. pom.xml (Maven)

```xml
<dependency>
    <groupId>org.ow2.asm</groupId>
    <artifactId>asm</artifactId>
    <version>9.6</version>
</dependency>
```

### 4. Deploy

```bash
mvn clean package
cp target/mein-mod.jar /pfad/zum/server/earlyplugins/
java -jar HytaleServer.jar --accept-early-plugins
```

---

## Event Patterns

### Einfaches Event

```java
eventRegistry.register(MyEvent.class, event -> {
    System.out.println("Event: " + event);
});
```

### Mit Priorität

```java
eventRegistry.register(EventPriority.EARLY, MyEvent.class, event -> {
    // wird vor NORMAL ausgeführt
});

eventRegistry.register(EventPriority.LAST, MyEvent.class, event -> {
    // wird als letztes ausgeführt
});
```

### Cancellable Event

```java
eventRegistry.register(DamageEvent.class, event -> {
    if (event instanceof ICancellable cancellable) {
        cancellable.setCancelled(true);  // verhindert Schaden
    }
});
```

### Async Event

```java
eventRegistry.registerAsync(AsyncEvent.class, future -> {
    return future.thenApply(event -> {
        // async processing
        return event;
    });
});
```

### Global Event (alle Keys)

```java
eventRegistry.registerGlobal(KeyedEvent.class, event -> {
    // erhält Events mit beliebigem Key
});
```

### Unhandled Event

```java
eventRegistry.registerUnhandled(MyEvent.class, event -> {
    // nur wenn kein anderer Handler registriert
});
```

### Registration entfernen

```java
EventRegistration<?, ?> reg = eventRegistry.register(MyEvent.class, handler);
reg.unregister();  // später entfernen
```

---

## Anchor UI Patterns (Update 3)

### Action registrieren (einfach)

```java
AnchorActionModule.get().register("my_action", (player, data) -> {
    String value = data.get("key").getAsString();
    // Client-Action verarbeiten
});
```

### Action registrieren (World-Thread-sicher)

```java
AnchorActionModule.get().register("my_world_action", (player, ref, store, data) -> {
    // Hier ist man im World-Thread → sicherer Zugriff auf ECS-Daten
    // ref = Entity-Referenz, store = ECS-Store
});
```

### Action aufräumen (shutdown)

```java
@Override
public void shutdown() {
    AnchorActionModule.get().unregister("my_action");
    AnchorActionModule.get().unregister("my_world_action");
}
```

---

## ECS Patterns

### Component lesen

```java
MyComponent comp = holder.getComponent(MY_COMPONENT_TYPE);
if (comp != null) {
    int value = comp.getValue();
}
```

### Component schreiben

```java
// Hinzufügen (fehler wenn existiert)
holder.addComponent(MY_TYPE, new MyComponent());

// Ersetzen (fehler wenn nicht existiert)
holder.replaceComponent(MY_TYPE, new MyComponent());

// Add oder Replace (sicher)
holder.putComponent(MY_TYPE, new MyComponent());
```

### Component entfernen

```java
// Wirft Exception wenn nicht vorhanden
holder.removeComponent(MY_TYPE);

// Sicher, gibt false zurück wenn nicht vorhanden
boolean removed = holder.tryRemoveComponent(MY_TYPE);
```

### Lazy Component

```java
// Erstellt Component nur wenn nicht vorhanden
holder.ensureComponent(MY_TYPE);

// Erstellen und zurückgeben
MyComponent comp = holder.ensureAndGetComponent(MY_TYPE);
```

### Eigene Component

```java
public class HealthComponent implements Component<EntityStore> {
    private int health;
    private int maxHealth;
    
    public HealthComponent(int max) {
        this.health = max;
        this.maxHealth = max;
    }
    
    public int getHealth() { return health; }
    public void setHealth(int h) { health = h; }
    
    @Override
    public Component<EntityStore> clone() {
        HealthComponent c = new HealthComponent(maxHealth);
        c.health = health;
        return c;
    }
}
```

---

## ASM Patterns

### Methode am Anfang modifizieren

```java
@Override
public byte[] transform(String className, String transformedName, byte[] bytes) {
    if (!className.equals("target/Class")) return null;
    
    ClassReader reader = new ClassReader(bytes);
    ClassWriter writer = new ClassWriter(ClassWriter.COMPUTE_FRAMES);
    
    ClassVisitor visitor = new ClassVisitor(Opcodes.ASM9, writer) {
        @Override
        public MethodVisitor visitMethod(int access, String name, 
                String desc, String signature, String[] exceptions) {
            MethodVisitor mv = super.visitMethod(access, name, desc, signature, exceptions);
            
            if (name.equals("targetMethod")) {
                return new MethodVisitor(Opcodes.ASM9, mv) {
                    @Override
                    public void visitCode() {
                        super.visitCode();
                        // Code am Methodenbeginn einfügen
                        mv.visitFieldInsn(Opcodes.GETSTATIC, 
                            "java/lang/System", "out", "Ljava/io/PrintStream;");
                        mv.visitLdcInsn("Method called!");
                        mv.visitMethodInsn(Opcodes.INVOKEVIRTUAL, 
                            "java/io/PrintStream", "println", "(Ljava/lang/String;)V", false);
                    }
                };
            }
            return mv;
        }
    };
    
    reader.accept(visitor, 0);
    return writer.toByteArray();
}
```

### Feld hinzufügen

```java
ClassVisitor visitor = new ClassVisitor(Opcodes.ASM9, writer) {
    @Override
    public void visitEnd() {
        // Neues Feld hinzufügen
        FieldVisitor fv = cv.visitField(
            Opcodes.ACC_PUBLIC, 
            "myField", 
            "I",  // int
            null, 
            null
        );
        fv.visitEnd();
        super.visitEnd();
    }
};
```

---

## Suche im Decompiled Code

### Klasse finden

```bash
find decompiled -name "ClassName.java"
```

### In Code suchen

```bash
grep -r "searchTerm" decompiled/com/hypixel/hytale/
```

### Methode finden

```bash
grep -r "methodName(" decompiled/com/hypixel/hytale/
```

### Events finden

```bash
find decompiled -name "*Event.java"
```

---

## Wichtige Pfade

```
Dekompilierter Code:
decompiled/com/hypixel/hytale/

Plugin API:          decompiled/com/hypixel/hytale/plugin/early/
Event System:        decompiled/com/hypixel/hytale/event/
ECS System:          decompiled/com/hypixel/hytale/component/
Protocol:            decompiled/com/hypixel/hytale/protocol/
Server Logic:        decompiled/com/hypixel/hytale/server/
Builtin Modules:     decompiled/com/hypixel/hytale/builtin/
```
