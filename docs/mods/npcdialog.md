# Mod: NPCDialog

**ID**: `npcdialog` (inferred)
**Version**: 1.2.2
**Package**: `com.hytale.npcdialog`
**Dependency**: `NPCDialog-1.2.2.jar`

## 1. Configuration Schema
Location: `mods/NPCDialog/`

### `NPCDialogConfig.json`
Global settings.
```json
{
  "enableEntityInteraction": true,
  "maxDialogPages": 20,
  "maxAnswersPerPage": 6,
  "maxDialogTextLength": 500
}
```

### `CommandsConfig.json`
Maps short aliases to required permissions.
```json
{
  "playerPermissionCommands": {
    "warps": "warp.list",
    "spawn": "spawn.use"
  }
}
```

## 2. NPC/Dialog Schema
Location: `mods/NPCDialog/npcs/{npc_id}.json` (Hypothetical path based on structure)
**Note**: The mod likely saves NPCs individually.

### Root Object (`NPC`)
```json
{
  "npcId": "UUID",
  "entityId": "UUID", // Links to spawned Hytale entity
  "name": "Guard",
  "displayTitle": "City Guard",
  "firstPageId": "UUID", // Start page
  "pages": [ /* See Page Object below */ ],
  "preventCloseUntilLastPage": false,
  "frozen": false,
  "nonHostile": false,
  "invulnerable": true,
  "enableIdleAnimation": false,
  "interactionHint": "Talk",
  "showNameplate": false,
  "nameplateText": "Guard"
}
```

### Page Object (`DialogPage`)
```json
{
  "pageId": "UUID",
  "title": "Welcome",
  "content": "Hello traveler.",
  "answers": [ /* DialogAnswer objects */ ],
  
  // Navigation Buttons
  "nextButtonText": "Next",
  "nextButtonCommand": "say Next clicked",
  "nextButtonRequirement": { /* Requirement Object */ },
  "nextButtonReward": { /* Reward Object */ },
  "isNextButtonLast": false,

  "previousButtonText": "Back",
  "enablePreviousButton": true,
  
  "closeButtonText": "Bye",
  
  "customButton1Text": "Trade",
  "customButton1Command": "shop open",
  "enableCustomButton1": true
}
```

### Check/Reward Objects
**Requirement**:
```json
{
  "type": "item_check", // or permission, quest, etc.
  "value": "Coin",
  "amount": 10,
  "inverted": false
}
```

**Reward**:
```json
{
  "type": "give_item",
  "value": "Apple",
  "amount": 5
}
```

## 3. Commands
Prefix: `/npcdialog` (Alias: `/npc`)

| Subcommand | Permission | Description |
| :--- | :--- | :--- |
| `create <name>` | `npcdialog.admin` | Create new NPC definition |
| `edit <name>` | `npcdialog.admin` | Open Editor GUI |
| `delete <name>` | `npcdialog.admin` | Delete NPC |
| `bind <npc_id>` | `npcdialog.admin` | Bind to looked-at entity |
| `reload` | `npcdialog.admin` | Reload configs |
| `list` | `npcdialog.admin` | List all NPCs |

## 4. Developer API
**Jar**: `NPCDialog-1.2.2.jar`
**Package**: `com.hytale.npcdialog`

### Interaction
```java
import com.hytale.npcdialog.NPCDialogPlugin;
import com.hytale.npcdialog.manager.NPCManager;
import com.hytale.npcdialog.model.NPC;

// 1. Get Manager
NPCManager manager = NPCDialogPlugin.getInstance().getNPCManager();

// 2. Get NPC
NPC npc = manager.getNPC(npcUuid);

// 3. Start Dialog Forcefully
manager.startDialog(player, npc);
```
