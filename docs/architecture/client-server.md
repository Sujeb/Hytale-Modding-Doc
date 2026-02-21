# Client-Server Architecture

> **CONTEXT**: conceptual understanding of Hytale's modding environment.
> **SOURCE**: Analysis of `Options.java`, `AssetModule`, and Networking.

## 1. The "Everything is a Server" Principle

In Hytale, your mod **always** runs on a Server.
*   **Dedicated Server**: A headless java process.
*   **Singleplayer**: The client spins up an internal, hidden set of server threads.

There is **no separate Client Modding API**. You do not write code that sits in the Client's `render()` loop.
Instead, you write Server code that sends instructions to the Client.

## 2. Bridging the Gap

How do you modify the client if code runs on the server?

### A. Asset Packs (Static)
The server sends `AssetPack`s to the client on join.
*   **What**: Textures, Models, Sounds, Particles, Shaders.
*   **Mechanism**: The client downloads identical assets to render new blocks/items consistent with the server's logic.

### B. UI Packets (Dynamic)
The server sends `CustomUICommand` packets.
*   **What**: Open windows, update text, show HUDs.
*   **Mechanism**: `UICommandBuilder` constructions are serialized to BSON and sent to the client, which constructs the DOM.

### C. Camera & Control
The server can force camera perspectives or input modes via packets.
*   **Example**: `CameraIO` packets can lock the camera or apply shakes.

## 3. Singleplayer Differences

While the API is the same, the environment differs slightly:
*   **Latency**: Zero in singleplayer.
*   **Asset Loading**: In Singleplayer/Dev, usage of `reload` commands might instantly reflect changes without a full rejoin.
*   **Paths**: Singleplayer worlds have their own `mods/` folder inside the save directory, distinct from the global `mods/` folder.

## 4. Best Practice

**Always write code assuming 500ms latency.**
Even if testing in Singleplayer, assume the client is remote. Use prediction (client-side prediction is built-in for movement) and do not rely on instant UI feedback loops that round-trip to the server unless necessary.
