# API Signatures Reference

> **LLM-optimiert**: Exakte Signaturen aus dekompiliertem Code

---

## IEventRegistry (24 Methoden)

Pfad: `decompiled/com/hypixel/hytale/event/IEventRegistry.java`

### Synchrone Events (ohne Key)

```java
// Standard-Registration
<EventType extends IBaseEvent<Void>> EventRegistration<Void, EventType> 
    register(Class<? super EventType> eventClass, Consumer<EventType> handler)

// Mit Priorität (Enum)
<EventType extends IBaseEvent<Void>> EventRegistration<Void, EventType> 
    register(EventPriority priority, Class<? super EventType> eventClass, Consumer<EventType> handler)

// Mit Priorität (short)
<EventType extends IBaseEvent<Void>> EventRegistration<Void, EventType> 
    register(short priority, Class<? super EventType> eventClass, Consumer<EventType> handler)
```

### Synchrone Events (mit Key)

```java
// Mit Key
<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    register(Class<? super EventType> eventClass, KeyType key, Consumer<EventType> handler)

// Mit Key + Priorität (Enum)
<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    register(EventPriority priority, Class<? super EventType> eventClass, KeyType key, Consumer<EventType> handler)

// Mit Key + Priorität (short)
<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    register(short priority, Class<? super EventType> eventClass, KeyType key, Consumer<EventType> handler)
```

### Async Events (ohne Key)

```java
<EventType extends IAsyncEvent<Void>> EventRegistration<Void, EventType> 
    registerAsync(Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)

<EventType extends IAsyncEvent<Void>> EventRegistration<Void, EventType> 
    registerAsync(EventPriority priority, Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)

<EventType extends IAsyncEvent<Void>> EventRegistration<Void, EventType> 
    registerAsync(short priority, Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)
```

### Async Events (mit Key)

```java
<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsync(Class<? super EventType> eventClass, KeyType key, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)

<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsync(EventPriority priority, Class<? super EventType> eventClass, KeyType key, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)

<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsync(short priority, Class<? super EventType> eventClass, KeyType key, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)
```

### Global Events

```java
<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerGlobal(Class<? super EventType> eventClass, Consumer<EventType> handler)

<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerGlobal(EventPriority priority, Class<? super EventType> eventClass, Consumer<EventType> handler)

<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerGlobal(short priority, Class<? super EventType> eventClass, Consumer<EventType> handler)

<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsyncGlobal(Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)

<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsyncGlobal(EventPriority priority, Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)

<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsyncGlobal(short priority, Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)
```

### Unhandled Events

```java
<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerUnhandled(Class<? super EventType> eventClass, Consumer<EventType> handler)

<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerUnhandled(EventPriority priority, Class<? super EventType> eventClass, Consumer<EventType> handler)

<KeyType, EventType extends IBaseEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerUnhandled(short priority, Class<? super EventType> eventClass, Consumer<EventType> handler)

<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsyncUnhandled(Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)

<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsyncUnhandled(EventPriority priority, Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)

<KeyType, EventType extends IAsyncEvent<KeyType>> EventRegistration<KeyType, EventType> 
    registerAsyncUnhandled(short priority, Class<? super EventType> eventClass, Function<CompletableFuture<EventType>, CompletableFuture<EventType>> handler)
```

---

## EventPriority

Pfad: `decompiled/com/hypixel/hytale/event/EventPriority.java`

```java
public enum EventPriority {
    FIRST(-21844),
    EARLY(-10922),
    NORMAL(0),
    LATE(10922),
    LAST(21844);
    
    short priority();
}
```

---

## Holder<ECS_TYPE> (ECS Entity Container)

Pfad: `decompiled/com/hypixel/hytale/component/Holder.java` (539 Zeilen, 23KB)

### Konstruktoren

```java
Holder()
Holder(ComponentRegistry<ECS_TYPE> registry)
Holder(ComponentRegistry<ECS_TYPE> registry, Archetype<ECS_TYPE> archetype, Component<ECS_TYPE>[] components)
```

### Component-Zugriff

```java
// Komponente abrufen (thread-safe read lock)
@Nullable <T extends Component<ECS_TYPE>> T getComponent(ComponentType<ECS_TYPE, T> componentType)

// Komponente hinzufügen (wirft Exception wenn schon vorhanden)
<T extends Component<ECS_TYPE>> void addComponent(ComponentType<ECS_TYPE, T> componentType, T component)

// Komponente ersetzen (wirft Exception wenn nicht vorhanden)
<T extends Component<ECS_TYPE>> void replaceComponent(ComponentType<ECS_TYPE, T> componentType, T component)

// Komponente hinzufügen oder ersetzen
<T extends Component<ECS_TYPE>> void putComponent(ComponentType<ECS_TYPE, T> componentType, T component)

// Komponente entfernen (wirft Exception wenn nicht vorhanden)
<T extends Component<ECS_TYPE>> void removeComponent(ComponentType<ECS_TYPE, T> componentType)

// Komponente entfernen, gibt false zurück wenn nicht vorhanden
<T extends Component<ECS_TYPE>> boolean tryRemoveComponent(ComponentType<ECS_TYPE, T> componentType)

// Komponente sicherstellen (lazy add)
<T extends Component<ECS_TYPE>> void ensureComponent(ComponentType<ECS_TYPE, T> componentType)

// Sicherstellen und zurückgeben
<T extends Component<ECS_TYPE>> T ensureAndGetComponent(ComponentType<ECS_TYPE, T> componentType)
```

### Archetype

```java
@Nullable Archetype<ECS_TYPE> getArchetype()

void init(Archetype<ECS_TYPE> archetype, Component<ECS_TYPE>[] components)
```

### Array Utilities

```java
Component<ECS_TYPE>[] ensureComponentsSize(int size)

static <T> Holder<T>[] emptyArray()
```

---

## Component<ECS_TYPE>

Pfad: `decompiled/com/hypixel/hytale/component/Component.java`

```java
public interface Component<ECS_TYPE> extends Cloneable {
    static final Component[] EMPTY_ARRAY = new Component[0];
    
    @Nullable Component<ECS_TYPE> clone();
    
    @Nullable default Component<ECS_TYPE> cloneSerializable() {
        return this.clone();
    }
}
```

---

## ClassTransformer

Pfad: `decompiled/com/hypixel/hytale/plugin/early/ClassTransformer.java`

```java
public interface ClassTransformer {
    // Priorität (höher = früher ausgeführt)
    default int priority() { return 0; }
    
    // Transform-Methode
    // Return null = keine Änderung
    // Return byte[] = modifizierter Bytecode
    @Nullable byte[] transform(
        @Nonnull String className,      // z.B. "com/hypixel/hytale/server/X"
        @Nonnull String transformedName,
        @Nonnull byte[] classBytes
    );
}
```

---

## Packet

Pfad: `decompiled/com/hypixel/hytale/protocol/Packet.java`

```java
public interface Packet {
    int getId();
    void serialize(ByteBuf buf);
    int computeSize();
}
```

---

## ICancellable

Pfad: `decompiled/com/hypixel/hytale/event/ICancellable.java`

```java
public interface ICancellable {
    boolean isCancelled();
    void setCancelled(boolean cancelled);
}
```

---

## IEvent / IAsyncEvent

```java
// Pfad: decompiled/com/hypixel/hytale/event/IEvent.java
public interface IEvent<KeyType> extends IBaseEvent<KeyType> { }

// Pfad: decompiled/com/hypixel/hytale/event/IAsyncEvent.java  
public interface IAsyncEvent<KeyType> extends IBaseEvent<KeyType> { }

// Pfad: decompiled/com/hypixel/hytale/event/IBaseEvent.java
public interface IBaseEvent<KeyType> { }
```

---

## Usage Patterns

### Event Registration
```java
// Einfachste Form
eventRegistry.register(MyEvent.class, event -> {
    // handle
});

// Mit Priorität
eventRegistry.register(EventPriority.EARLY, MyEvent.class, event -> {
    // handle early
});

// Cancellable Event
eventRegistry.register(DamageEvent.class, event -> {
    if (event instanceof ICancellable c) {
        c.setCancelled(true);
    }
});
```

### Component Access
```java
// Lesen
MyComponent comp = holder.getComponent(MY_COMPONENT_TYPE);
if (comp != null) {
    // use
}

// Schreiben (add or replace)
holder.putComponent(MY_COMPONENT_TYPE, new MyComponent());

// Lazy Get
MyComponent comp = holder.ensureAndGetComponent(MY_COMPONENT_TYPE);
```

### ClassTransformer
```java
public class MyTransformer implements ClassTransformer {
    @Override
    public int priority() { return 100; }
    
    @Override
    public byte[] transform(String className, String transformedName, byte[] bytes) {
        if (!className.equals("target/Class")) return null;
        // Use ASM/ByteBuddy to modify bytes
        return modifiedBytes;
    }
}
```

---

## AnchorActionModule (Update 3)

Pfad: `decompiled/com/hypixel/hytale/server/core/modules/anchoraction/AnchorActionModule.java`

```java
public class AnchorActionModule extends JavaPlugin {
    public static final PluginManifest MANIFEST;

    public static AnchorActionModule get();
    
    // Action-Handler registrieren
    public void register(@Nonnull String action, @Nonnull AnchorActionHandler handler);
    public void register(@Nonnull String action, @Nonnull WorldThreadAnchorActionHandler handler);
    public void unregister(@Nonnull String action);
    
    // Intern: Client-Anfrage verarbeiten
    public boolean tryHandle(@Nonnull PlayerRef playerRef, @Nonnull String rawData);
    
    // Handler-Interfaces
    @FunctionalInterface
    public interface AnchorActionHandler {
        void handle(@Nonnull PlayerRef player, @Nonnull JsonObject data);
    }
    
    @FunctionalInterface
    public interface WorldThreadAnchorActionHandler {
        void handle(@Nonnull PlayerRef player, @Nonnull Ref<EntityStore> ref, 
                    @Nonnull Store<EntityStore> store, @Nonnull JsonObject data);
    }
}
```

---

## PluginBase Codec Registries (Update 3)

Pfad: `decompiled/com/hypixel/hytale/server/core/plugin/PluginBase.java`

```java
// Drei neue getCodecRegistry() Überladungen:

@Nonnull
<T, C extends Codec<? extends T>> CodecMapRegistry<T, C> 
    getCodecRegistry(@Nonnull StringCodecMapCodec<T, C> mapCodec);

@Nonnull
<K, T extends JsonAsset<K>> CodecMapRegistry.Assets<T, ?> 
    getCodecRegistry(@Nonnull AssetCodecMapCodec<K, T> mapCodec);

@Nonnull
<V> MapKeyMapRegistry<V> 
    getCodecRegistry(@Nonnull MapKeyMapCodec<V> mapCodec);

// Async config preload:
@Nullable
CompletableFuture<Void> preLoad();
```
