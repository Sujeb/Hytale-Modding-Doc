# Mod: KyuubiSoft Achievements

**ID**: `kyuubisoft_achievements` (inferred)
**Version**: 1.9.0
**Package**: `com.hytale.achievements`
**Dependency**: `KyuubiSoftAchievements-1.9.0.jar`

## 1. Configuration Schema
Location: `mods/KyuubiSoftAchievements/configs/`

### `config.json`
Global settings for the mod.
```json
{
  "language": "en-US",
  "loadDefaultAchievements": true,
  "loadMmoAchievements": true,
  "loadRpgLevelingAchievements": true,
  "display": {
    "displayMode": "both", // "chat", "action_bar", "both"
    "titlePosition": "prefix",
    "nametagFormat": "{title}\n{name}",
    "chatFormat": "[{title}] {name}: {message}",
    "titleColor": "#FFD700",
    "showRpgLevel": false,
    "rpgLevelFormat": "Lv.{level}",
    "rpgLevelColor": "#55FFFF",
    "rpgLevelPosition": "after_name"
  },
  "notifications": {
    "enabled": true,
    "displayMode": "chat",
    "displayDurationMs": 5000,
    "showRewardsInBanner": true,
    "broadcastToChat": false,
    "broadcastFormat": "{player} unlocked: {achievement}"
  },
  "tracking": {
    "saveIntervalSeconds": 300,
    "enableExploration": true,
    "enableCombat": true,
    "enableProgression": true,
    "enableSocial": true,
    "ignoreCreativeMode": true
  },
  "admin": {
    "cascadeRevoke": false,
    "cascadeRevokeWarning": true,
    "bypassPrerequisites": false
  },
  "export": {
    "enabled": true,
    "intervalSeconds": 300,
    "exportGlobalStats": true,
    "exportLeaderboards": true,
    "exportRecentUnlocks": true
  }
}
```

## 2. Achievement Schema
Location: `custom/custom_achievements.json`
Use this schema to generate new achievements.

```json
{
  "achievements": [
    {
      "id": "unique_id_string",
      "category": "progression", // standard: combat, exploration, social, progression
      "icon": "path/to/icon.png", // Start with "achievements/icons/"
      "iconItem": "Hytaleitem_Name", // Optional override
      "hidden": false,
      "difficulty": "normal", // normal, epic, legendary
      "title": {
        "id": "title_id",
        "color": "#HEXCODE"
      },
      "trigger": {
        "type": "blocks_mined", 
        // Types: kills, blocks_mined, zones_discovered, distance_walked, chat_messages, playtime_minutes, manual, auto_on_prerequisites
        "target": "any", // or specific entity/block ID
        "count": 100
      },
      "rewards": [
        {
          "type": "item",
          "itemId": "Item_Name",
          "amount": 1
        },
        {
          "type": "xp", // if RPGLeveling enabled
          "amount": 500
        },
        {
          "type": "command",
          "command": "give {player} Sword 1"
        }
      ],
      "requires": ["previous_achievement_id"], // List or String
      "showRequires": true,
      "additionalInfo": {
        "text": "Description text",
        "color": "#FFFFFF"
      }
    }
  ]
}
```

## 3. Commands
Prefix: `/achievements` (Alias: `/ach`)

| Subcommand | Permission | Description |
| :--- | :--- | :--- |
| `list` | (None) | View achievements GUI |
| `stats` | (None) | View personal stats |
| `admin grant <player> <id>` | `achievements.admin` | Force unlock achievement |
| `admin revoke <player> <id>` | `achievements.admin` | Revoke achievement |
| `admin reload` | `achievements.admin` | Reload configs |
| `titles` | (None) | Open Title Selector |
| `rewards` | (None) | Open Rewards GUI |
| `lootbags` | (None) | Open Lootbags GUI |

## 4. Developer API
**Jar**: `KyuubiSoftAchievements-1.9.0.jar`
**Package**: `com.hytale.achievements`

### Interaction
```java
import com.hytale.achievements.AchievementPlugin;
import com.hytale.achievements.api.AchievementAPI;
import com.hytale.achievements.manager.PlayerAchievementManager;

// 1. Get API
AchievementAPI api = AchievementPlugin.getInstance().getApi();

// 2. Unlock Achievement (Code Trigger)
api.unlockAchievement(player, "my_custom_achievement");

// 3. Check Status
boolean hasUnlocked = api.hasUnlocked(player, "some_id");

// 4. Custom Events
// Post events to the EventBus found in 'com.hytale.achievements.events'
```
