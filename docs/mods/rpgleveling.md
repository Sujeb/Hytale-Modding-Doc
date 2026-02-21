# Mod: RPGLeveling

**ID**: `rpgleveling` (inferred)
**Version**: 0.2.6
**Package**: `org.zuxaw.plugin`
**Dependency**: `RPGLeveling-0.2.6.jar`

## 1. Configuration Schema
Location: `mods/RPGLeveling/`

### `LevelingConfig.json`
Controls core progression logic.
```json
{
  "Version": "0.2.6",
  "MaxLevel": 100,
  "RateExp": 3.0,
  "BaseXP": 1.0,
  "LevelBaseXP": 150.0,
  "LevelOffset": 0.0,
  "EnableHUD": true,
  "Debug": false,
  "ResetLevelOnDeath": false,
  "BlacklistedEntities": "", // Comma-separated list (e.g. "Sheep,Pig")
  "EnablePVPXp": false,
  "EnableMonsterLevelScale": true,
  "DifficultyPreset": "normal", // Options: easy, normal, hard, extreme
  "EnableGapLevelDefense": true,
  "EnableGapLevelDamageToPlayer": true,
  "EnableGapLevelXpReducer": true,
  "EnablePlayerLevelNameplate": true,
  "EnableNameplateLevelDisplay": true,
  "EnableNotification": true,
  "EnableCurrentZoneHudDisplay": true,
  "EnableBlockXP": true,
  "BlockXPBaseMining": 5.0,
  "BlockXPBaseWood": 3.0,
  "BlockXPPlayerLevelFactor": 0.1
}
```

### `StatsLevelConfig.json`
Controls attribute scaling.
```json
{
  "Version": "0.2.6",
  "StatPointsPerLevel": 5,
  "StatValuePerPoint": 1.0, // Generic multiplier?
  "HealthStatValuePerPoint": 1.0,
  "StaminaStatValuePerPoint": 1.0,
  "ManaStatValuePerPoint": 1.0,
  "AmmoStatValuePerPoint": 1.0,
  "OxygenStatValuePerPoint": 1.0,
  "DamageStatValuePerPoint": 1.5,
  "CriticalDamageStatValuePerPoint": 0.2,
  "MiningStatValuePerPoint": 0.5,
  "WoodcuttingStatValuePerPoint": 0.5,
  "DefenseStatValuePerPoint": 1.5,
  "DefenseMaxReductionRatio": 0.8,
  "MiningPerTickDivisor": 100.0,
  "StaminaConsumptionValuePerPoint": 0.01,
  "StaminaRegenSpeedValuePerPoint": 0.18,
  // Max Point Caps
  "MaxStatPointsHealth": 50,
  "MaxStatPointsStamina": 50,
  "MaxStatPointsMana": 50,
  "MaxStatPointsDamage": 50,
  "MaxStatPointsDefense": 50,
  // ... and others (refer to params above)
  "BlacklistedStats": "" // e.g. "StaminaRegenDelay"
}
```

## 2. Commands
Prefix: `/lvl`

| Subcommand | Permission | Description |
| :--- | :--- | :--- |
| `gui` | (None) | Open Stats GUI |
| `info` | (None) | Show stats in chat |
| `setlevel <player> <int>` | `hytale.command.admin` (OP) | Set exact level |
| `setpoints <player> <int>` | `hytale.command.admin` (OP) | Set unspent points |
| `addxp <player> <double>` | `hytale.command.admin` (OP) | Give XP |
| `resetstats <player>` | `hytale.command.admin` (OP) | Refund all points |
| `reload` | `hytale.command.admin` (OP) | Reload JSON configs |

## 3. Developer API
**Jar**: `RPGLeveling-0.2.6.jar`
**Package**: `org.zuxaw.plugin`

### Accessing Player Data
```java
import org.zuxaw.plugin.RPGLevelingPlugin;
import org.zuxaw.plugin.manager.PlayerManager;
import org.zuxaw.plugin.model.RPGPlayer;

// 1. Get Manager
PlayerManager pm = RPGLevelingPlugin.getInstance().getPlayerManager();

// 2. Get Player Object
RPGPlayer rpgPlayer = pm.getPlayer(hytalePlayer); // 'hytalePlayer' is Entity or Player class

// 3. Read/Write Data
int level = rpgPlayer.getLevel();
rpgPlayer.addXp(500.0);
rpgPlayer.setStat("strength", 10);
```
