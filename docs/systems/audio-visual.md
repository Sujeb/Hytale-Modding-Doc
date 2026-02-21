# Audio & Visual Systems

> **CONTEXT**: Documentation for Sounds and Particles.
> **SOURCE**: `com.hypixel.hytale.protocol.SoundEvent`, `com.hypixel.hytale.protocol.ParticleSystem`, `com.hypixel.hytale.server.core.universe.world.SoundUtil`.

## 1. Sound System

Hytale uses a data-driven sound system based on `SoundEvent` assets.

### A. Sound Event Asset (`sfx.json`)
Sounds are defined in JSON and can have multiple "layers" (randomized samples).

```json
{
  "name": "my_mod:magic_spell_cast",
  "volume": 1.0,
  "pitch": 1.0,
  "maxDistance": 32.0,
  "audioCategory": "SFX",
  "layers": [
    {
      "sound": "my_mod:audio/spell_cast_1.ogg",
      "volume": 0.8
    },
    {
      "sound": "my_mod:audio/spell_cast_2.ogg",
      "volume": 0.9
    }
  ]
}
```

### B. Playing Sounds in Code
Use **`SoundUtil`** for easy playback. You need the **Integer ID** of the sound from the registry (AssetMap), not the string name directly.

> **Tip**: Cache the ID during startup (`AssetBundle.findAssetId`).

```java
int soundId = SoundEvent.getAssetMap().getId("my_mod:magic_spell_cast");

// Play 3D Sound at location
SoundUtil.playSoundEvent3d(
    soundId,
    SoundCategory.SFX,
    x, y, z,
    componentAccessor // EntityStore accessor
);

// Play 2D Sound (Global/UI) to specific player
SoundUtil.playSoundEvent2dToPlayer(
    playerRef, 
    soundId, 
    SoundCategory.UI
);
```

## 2. Particle System

Particles are complex visual effects defined in assets and spawned via packets.

### A. Particle System Asset (`particle_system.json`)
```json
{
  "id": "my_mod:magic_sparkles",
  "lifeSpan": 5.0,
  "spawners": [
    {
      "type": "POINT",
      "rate": 10,
      "particle": "my_mod:sparkle_texture"
    }
  ]
}
```

### B. Spawning Particles
To spawn a particle system, you generally send a **Packet**.

```java
// Spawn at location
SpawnParticleSystem packet = new SpawnParticleSystem(
    "my_mod:magic_sparkles",
    new Position(x, y, z),
    1.0f // Scale
);

// Send to all players in range
PlayerUtil.broadcastPacketToPlayers(componentAccessor, packet);
```

## 3. Client Limitations
*   You **cannot** upload custom shaders (yet).
*   Models and Animations are data-driven (JSON + glTF/Hym).
*   Interactive UI is done via `CustomUIPage` (see UI docs).
