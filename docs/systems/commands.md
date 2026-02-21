# Command System Specification

> **CONTEXT**: LLM Reference for `com.hypixel.hytale.server.core.command.system.CommandManager`.
> **SOURCE**: Decompiled `CommandManager.java`.

## 1. System Overview

**Manager**: `com.hypixel.hytale.server.core.command.system.CommandManager`
**Base Class**: `com.hypixel.hytale.server.core.command.system.AbstractCommand`

## 2. Registration & Basic Command

Commands extend `AbstractCommand` and are registered via `getCommandRegistry().register()`.

```java
public class MyCommand extends AbstractCommand {
    public MyCommand() {
        super("mycmd", "Description of my command", false); // name, description, confirmationRequired
    }

    @Override
    protected CompletableFuture<Void> execute(CommandContext ctx) {
        ctx.sender().sendMessage("Hello from MyCommand!");
        return CompletableFuture.completedFuture(null);
    }
}

// Registration in plugin start()
this.getCommandRegistry().register(new MyCommand());
```

## 3. Key Concepts

### CommandSender
Interface implemented by `Player`, `Console`, etc.
- `sendMessage(String/Message)`
- `hasPermission(String)`

### CommandManager
Singleton: `CommandManager.get()`
Methods:
- `register(AbstractCommand command)`
- `handleCommand(CommandSender sender, String command)`

## 4. AbstractCommand Internals

Under the hood, `AbstractCommand` uses `acceptCall()` as its abstract execution entry point.
The convenience `execute(CommandContext)` method shown above wraps this. For advanced cases:

```java
public abstract class AbstractCommand {
    private final String name;
    private final Set<String> aliases;

    // Low-level entry point (prefer overriding execute(CommandContext) instead)
    public abstract CompletableFuture<Void> acceptCall(CommandSender sender, ParserContext context, ParseResult result);
}
```
## 5. AbstractCommand API Details

### Constructor
```java
public MyCommand() {
    super("mycommand", "Description", false); // name, desc, confirmationRequired
}
```

### Arguments
Register arguments in the constructor:
```java
// Required Argument
this.withRequiredArg("player", "Target player", ArgumentType.PLAYER);

// Optional Argument
this.withOptionalArg("count", "Item count", ArgumentType.INTEGER);

// Flag Argument
this.withFlagArg("force", "Force execution");
```

### Subcommands
```java
this.addSubCommand(new MySubCommand());
```

### Execution
```java
@Override
protected CompletableFuture<Void> execute(CommandContext ctx) {
    CommandSender sender = ctx.sender();
    String targetName = ctx.getArgument("player");
    
    // Logic...
    return CompletableFuture.completedFuture(null);
}
```

### Permissions
Permissions are auto-generated based on command structure (e.g., `hytale.command.mycommand.subcommand`), or can be set manually:
```java
this.requirePermission("my.custom.node");
```
*(Note: Simple mods typically check `AbstractCommand` details for simpler `execute` abstractions if available, otherwise implement `acceptCall`)*
