# Mod: NPCQuests

**ID**: `npcquests` (inferred)
**Version**: 1.0.2
**Package**: `com.hytale.npcquests`
**Dependency**: `NPCQuests-1.0.2.jar`
**Requires**: `NPCDialog`

## 1. Overview
**Type**: Logic Extension
**Storage**: `mods/NPCQuests/questProgress.json` (Player Data)

This mod does **not** have quest definition files.
Quests are constructed entirely within `NPCDialog` pages using **Custom Requirements** and **Custom Rewards**.
The mod simply tracks the state (`NOT_STARTED`, `ACTIVE`, `COMPLETED`) of arbitrary Quest IDs.

## 2. Integration Protocol (NPCDialog)

### Requirements
Use these in `NPCDialog` Page/Button `customRequirements`.

| Requirement ID | Description |
| :--- | :--- |
| `quest:completed:{quest_id}` | Check if quest is `COMPLETED`. |
| `quest:active:{quest_id}` | Check if quest is `ACTIVE`. |
| `quest:status:{quest_id}:{STATUS}` | Check for specific status (e.g. `NOT_STARTED`, `FAILED`). |

### Rewards
Use these in `NPCDialog` Page/Button `customRewards`.

| Reward ID | Description |
| :--- | :--- |
| `quest:start:{quest_id}` | Sets status to `ACTIVE`. |
| `quest:complete:{quest_id}` | Sets status to `COMPLETED`. |
| `quest:set_status:{quest_id}:{STATUS}` | Sets arbitrary status. |

## 3. Example Quest Flow (JSON)
**Step 1: Quest Giver (Start)**
```json
{
  "title": "Quest Giver",
  "content": "Can you bring me 5 Apples?",
  "nextButtonText": "I'll do it!",
  "nextButtonReward": {
    "customRewards": [
      {
        "rewardId": "quest:start:apple_collection",
        "amount": 1
      }
    ]
  }
}
```

**Step 2: Quest Turn-In (Complete)**
```json
{
  "title": "Quest Giver",
  "content": "Do you have them?",
  "nextButtonText": "Here you go.",
  "nextButtonRequirement": {
    "itemRequirements": [ { "itemId": "Apple", "quantity": 5, "consume": true } ],
    "customRequirements": [
      { "requirementId": "quest:active:apple_collection", "amount": 1 }
    ]
  },
  "nextButtonReward": {
    "customRewards": [
      { "rewardId": "quest:complete:apple_collection", "amount": 1 },
      { "rewardId": "give_item:Sword", "amount": 1 } // Hypothetical item reward
    ]
  }
}
```

## 4. Commands
Prefix: `/quests`

| Subcommand | Permission | Description |
| :--- | :--- | :--- |
| `admin set <player> <quest> <status>` | `npcquests.admin` | Force set status |
| `admin check <player> <quest>` | `npcquests.admin` | Check status |
| `log` | (None) | View active quests |

## 5. Developer API
**Jar**: `NPCQuests-1.0.2.jar`
**Package**: `com.hytale.npcquests`

### Quest Manager
```java
import com.hytale.npcquests.NPCQuestsPlugin;
import com.hytale.npcquests.manager.QuestTreeManager;
import com.hytale.npcquests.domain.quest.QuestStatus;

// 1. Get Manager
QuestTreeManager manager = NPCQuestsPlugin.getInstance().getQuestTreeManager();

// 2. Check Status
boolean isCompleted = manager.isQuestCompleted(playerUuid, "apple_collection");

// 3. Set Status
manager.setQuestStatus(playerUuid, "apple_collection", QuestStatus.COMPLETED);
```
