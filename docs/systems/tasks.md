# Task Schedulers (TaskRegistry)

> **Source**: `com.hypixel.hytale.server.core.task.TaskRegistry`
> **Status**: Verified against Decompiled Server Code (February 2026).

The `TaskRegistry` allows plugins to register tasks for lifecycle tracking and management.

## 1. Accessing the Registry

```java
// Via PluginBase
TaskRegistry tasks = this.getTaskRegistry();
```

## 2. Registering Tasks

The `TaskRegistry` provides two registration methods:

### CompletableFuture Tasks
```java
CompletableFuture<Void> myTask = CompletableFuture.runAsync(() -> {
    // Async-Arbeit (z.B. Datenbank, HTTP)
});
tasks.registerTask(myTask);
```

### ScheduledFuture Tasks
```java
// Via Java ScheduledExecutorService
ScheduledExecutorService executor = Executors.newSingleThreadScheduledExecutor();
ScheduledFuture<Void> repeating = executor.scheduleAtFixedRate(
    () -> { /* wiederholte Arbeit */ },
    0, 5, TimeUnit.SECONDS
);
tasks.registerTask(repeating);
```

## 3. TaskRegistration

Registered tasks are tracked as `TaskRegistration` objects extending the base `Registration` class, allowing the plugin system to manage and clean up tasks on shutdown.

## 4. Hinweise

> **WARNUNG**: Der `TaskRegistry` ist *kein* Minecraft-artiger Scheduler mit `runLater()` oder `runRepeating()`. Er dient zur Registrierung und Verwaltung von Java-Standard-Futures.

- Tasks werden beim Plugin-Shutdown automatisch bereinigt.
- Für verzögertes Ausführen: Standard-Java `CompletableFuture.delayedExecutor()` verwenden.
- Für periodische Aufgaben: `ScheduledExecutorService` verwenden.
- Thread-Sicherheit beachten: Hytale-API (Entities, Blocks) **nicht** aus Async-Threads aufrufen!

