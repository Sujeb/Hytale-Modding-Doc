<div align="center">
  <h1>Hytale Modding Documentation</h1>
  <p><strong>An unofficial, AI-optimized reference for Hytale Modding</strong></p>
  <p>
    <a href="https://github.com/Sujeb/hytale-modding-doc/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="License"></a>
    <img src="https://img.shields.io/badge/Hytale-Update_3_(2026.02.17--255364b8e)-brightgreen" alt="Hytale Update 3">
    <img src="https://img.shields.io/badge/Target-AI_Agents_%26_Developers-orange" alt="Target">
  </p>
</div>

> [!WARNING]
> **AI-Slop Notice**
> The documentation, structure, and code references in this repository have been generated and formatted entirely by AI (Large Language Models) using Google Antigravity. While we strive for accuracy by cross-referencing decompiled server code, please verify critical implementations manually. This repository is provided "as is" and serves primarily as a contextual resource for other AI agents to learn from.

---

## 📖 About This Project

This repository serves as a verified documentation hub and reference point for **Hytale Modding**. It contains decompiled class structures, API references, and system behaviors (e.g., ECS, Blocks, UI).

> **Hytale Version:** Update 3 — Build `2026.02.17-255364b8e`

The primary focus of this repository is to be **AI-optimized**. The formatting is explicitly designed to be easily ingested by Large Language Models (LLMs) and AI agents (like GitHub Copilot, Cursor, or specialized coding assistants) to help developers quickly build plugins and scale their Hytale servers.

> **Note:** All documentation is cross-referenced with actual decompiled server code (Server 1.0+).

---

## 📂 Directory Layout

*   📁 **`docs/`** - Documentation Root
    *   `architecture/` - Core Server Systems (Client/Server sync, ECS)
    *   `systems/` - Subsystems (UI, WorldGen, Networking, Blocks, etc.)
    *   `reference/` - Lookup Tables (Commands, Events, Packets)
    *   `mods/` - Third-Party Mod Documentation (Decompiled & Verified)
    *   `libraries/` - Core Libraries (Netty, Gson, Hyxin)
*   📁 **`decompiled/`** - Workspace for raw decompiled artifacts (ignored in Git via `.gitkeep`)

---

## ⚙️ Core Systems (Quick Links)

| System | Documentation Path | Key Classes |
| :--- | :--- | :--- |
| **Plugin Loading** | [`getting-started.md`](docs/modding/getting-started.md) | `JavaPlugin`, `PluginBase` |
| **Networking** | [`networking.md`](docs/systems/networking.md) | `PacketRegistry`, `NetworkChannel` |
| **Entities (ECS)** | [`ecs.md`](docs/systems/ecs.md) | `Entity`, `Component`, `System` |
| **UI** | [`ui.md`](docs/systems/ui.md) | `UICommandBuilder`, `CustomUIPage` |
| **Commands** | [`commands.md`](docs/systems/commands.md) | `AbstractCommand`, `CommandManager` |
| **Audio & Visual** | [`audio-visual.md`](docs/systems/audio-visual.md) | `SoundEvent`, `ParticleSystem` |

---

## 🧩 Verified Third-Party Mods

This repository also contains reverse-engineered documentation for several popular community mods to serve as reference implementations:

| Mod ID | Documentation | Package Base |
| :--- | :--- | :--- |
| `rpgleveling` | [`rpgleveling.md`](docs/mods/rpgleveling.md) | `org.zuxaw.plugin.rpgleveling` |
| `kyuubisoft_achievements` | [`kyuubisoft-achievements.md`](docs/mods/kyuubisoft-achievements.md) | `com.hytale.achievements` |
| `npcdialog` | [`npcdialog.md`](docs/mods/npcdialog.md) | `com.hytale.npcdialog` |
| `npcquests` | [`npcquests.md`](docs/mods/npcquests.md) | `com.hytale.npcquests` |

---

## 🔍 Verification Protocol for Contributors / AI

If you are an AI assistant using this repository:
1. All docs **MUST** be verified against decompiled sources.
2. **Do not hallucinate APIs.** Check the provided `docs/` files or the raw decompiled output structure.
3. Pay attention to `> **CONTEXT**:` and `> **SOURCE**:` tags inside the Markdown files.

---

## 🔧 How to Decompile `HytaleServer.jar`

To generate the source reference files used in this documentation, you need to decompile the server jar. We recommend using **CFR**.

1. Download the latest `cfr.jar` from the [CFR Releases page](https://github.com/leibnitz27/cfr/releases).
2. Place both `HytaleServer.jar` and `cfr.jar` in the root directory of this repository.
3. Run the following command in your terminal:
   ```bash
   java -jar cfr.jar HytaleServer.jar --outputdir decompiled
   ```
4. All decompiled `.java` files will now be available in the `decompiled/` directory.

> **Note:** Both `HytaleServer.jar` and `cfr.jar`, as well as the contents of the `decompiled/` folder, are intentionally ignored by `.gitignore` to prevent distributing proprietary code.

---

## ⚖️ License & Disclaimer

This project is licensed under the [Apache License 2.0](LICENSE). 

_This product includes software concepts and decompiled references from Hytale Server, which is property of Hypixel Studios. It also contains documentation for various community-created Hytale mods. This project is an unofficial community resource and is not affiliated with, maintained, authorized, endorsed or sponsored by Hypixel Studios or the respective mod creators._ (See the [NOTICE](NOTICE) file for more details).
