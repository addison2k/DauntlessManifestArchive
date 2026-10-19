# Minecraft blocks and HUD inside Valheim

Two small mods that talk over a local connection. The full plan is in [docs/HANDOFF.md](docs/HANDOFF.md).

**Where this repo is:** milestone 3 of 8, frame export. Minecraft's camera copies Valheim's, and every Minecraft frame is written to a shared file as three planes: the world, its depth, and the HUD. Valheim can dump one frame to PNGs to prove it. Nothing is drawn in Valheim from Minecraft yet; that is milestone 4.

```
valheim-plugin/    C# BepInEx plugin   -> McBridge.dll
minecraft-mod/     Fabric mod (Java)   -> mcbridge-0.1.0.jar
docs/HANDOFF.md    the plan
```

## Pinned versions

The Valheim and Unity rows come from the A2K projects. Confirm them the first time the bridge connects (both are printed for you) and turn off auto-update for Valheim in Steam while you work on this.

| Thing | Version |
| --- | --- |
| Valheim | 1.0 |
| Unity (Valheim's) | 6000.0.61f1, the editor the A2K bundles are built with; needed for the shader in milestone 5 |
| BepInExPack Valheim | 5.4.2351 |
| Minecraft Java Edition | 1.21.1 |
| Fabric Loader | 0.19.5 or newer |
| Fabric API | 0.116.8+1.21.1 |
| Java | 25 to build (Fabric Loom 1.18 needs it; Gradle fetches it itself), 21 to run (ATLauncher's own) |
| Bridge protocol | 3 |

## One-time setup

You need two build tools. Neither game's launcher provides them.

1. **.NET SDK** (8 or newer), to build the Valheim plugin. Get it from <https://dotnet.microsoft.com/download>. Visual Studio is not needed.
2. **Any JDK 17 or newer**, to start the Minecraft mod's build. The build tool (Fabric Loom 1.18) only runs on Java 25, so `minecraft-mod/gradle/gradle-daemon-jvm.properties` tells Gradle to use a JDK 25: an installed one if it finds it, otherwise it downloads Temurin 25 into its own cache on the first build. Nothing is installed system-wide and `JAVA_HOME` is left alone. The mod itself is still compiled for Java 21. ATLauncher's own Java is only for running the game.

Open a new terminal afterwards and check that `dotnet --version` and `java -version` both answer.

## Build and install the Valheim plugin

1. Close Valheim.
2. In `valheim-plugin/`, run the same command as the A2K plugins:

   ```
   dotnet build -c Release -p:VALHEIM_INSTALL="C:\Program Files (x86)\Steam\steamapps\common\Valheim"
   ```

The build compiles against the game's own DLLs and copies `McBridge.dll` into `BepInEx/plugins/McBridge/` in the Valheim folder. Without `VALHEIM_INSTALL` it finds Valheim through Steam. If BepInEx lives somewhere else (a mod manager profile), set `BepInExDir` in `Paths.props`.

## Build and install the Minecraft mod

1. In `minecraft-mod/`, run:

   ```
   gradlew build
   ```

   The first run downloads Gradle, a JDK 25 and Minecraft, and takes several minutes.
2. In ATLauncher, create a **Vanilla Minecraft 1.21.1** instance with the **Fabric** loader.
3. On that instance, use **Add Mods** to install **Fabric API**.
4. Use **Open Folder**, go into `mods`, and copy in `minecraft-mod/build/libs/mcbridge-0.1.0.jar`. Take the plain jar, not the `-sources` one.

## Minecraft world

Milestone 2 onwards needs the empty world from the handoff. A normal world has its own terrain in the way.

Create New World, game mode **Creative**. On the World tab pick **Superflat**, then Customize, Presets, **The Void**.

That is all. The first time the world loads, the mod sets it up and says so in chat:

```
[McBridge] World prepared: removed the start platform at y -61, time fixed at noon, weather off, mob spawning off.
```

It also keeps the player hovering at all times, with or without Valheim running, so there is nothing to fall off. The mod only touches Superflat worlds, and it finds the platform by its shape (a stone square with one cobblestone in the middle at x 8, z 8), not by a fixed height.

It also turns off "pause on lost focus", because Minecraft never has focus while you play Valheim.

## Milestone 2 check: camera sync

You need to see both windows at once, so run Valheim windowed or put Minecraft on a second monitor. Make the Minecraft window roughly the same shape as Valheim's (16:9). Exact size matching comes in milestone 3.

1. Start both games and load both worlds, in either order. Chat shows the green "Connected to Valheim" line, and the Minecraft view jumps to empty sky.
2. In Valheim, aim at the ground a few metres away and press **F8**. Four cubes appear: white, with red, green and blue touching it.
3. The Minecraft window shows four concrete blocks in the same colours, in the same place on screen, at the same size.
4. Walk and orbit around the marker in Valheim. The Minecraft picture should be the Valheim picture with everything except the cubes removed. Press F8 again somewhere else to move it.

The colours are the test. Red is one metre along Valheim's +X, green is up, blue is along +Z. A mapping mistake shows up like this:

| What you see in Minecraft | What is wrong |
| --- | --- |
| Red and blue on swapped sides (a mirror image) | The Z flip |
| Marker slides the opposite way when you turn | Yaw sign or the 180 degree offset |
| Right place when level, drifts when you look up or down | Pitch |
| Right direction, wrong size, more wrong toward the edges | Field of view, or the window shapes differ |
| Blocks sit half a block off the cubes | Block-centre snapping |
| Nothing there, chat says the marker is outside the height range | `Coords.Y_OFFSET` needs changing |
| Blocks lag behind while turning, then catch up | Expected. That is the frame lag in the handoff's risk table. |

The cubes in Valheim are flat-coloured with no shadows and no collision. They vanish when you leave the world. The F8 key is `[Test] PlaceMarkerKey` in `BepInEx/config/dev.mcbridge.valheim.cfg`.

Passed on Oct 1, 2026: both windows matched to within a few pixels.

## Milestone 3 check: frame export

While Valheim is connected, the Minecraft window changes: it shows only the HUD on a black background, with no hand. That is expected. The world is read out before the HUD is drawn and then wiped, so that the HUD lands on a transparent background of its own. Disconnect Valheim and the window goes back to normal.

Use Fancy or Fast graphics in Minecraft, not Fabulous.

1. Start both games and load both worlds. Minecraft chat says "Exporting frames to Valheim".
2. In Valheim, press **F8** to place the marker somewhere in view. Stand still.
3. Press **F9**. Four PNGs are written to `BepInEx\mcbridge-frames\`:

   | File | Should show |
   | --- | --- |
   | `world.png` | The four marker blocks where they are on your Valheim screen, on a transparent background. No sky. |
   | `overlay.png` | The hotbar and crosshair on a transparent background. Creative mode has no hearts or hunger bar. |
   | `depth.png` | The blocks in grey, lighter when closer, on a transparent background. |
   | `combined.png` | The overlay on top of the world. |

4. `BepInEx\LogOutput.log` gets two lines. The first says how big the frame was and how many Valheim frames old it was. The second is a depth check: Valheim's own distance to the marker next to Minecraft's depth at the same pixel. Minecraft's number should be 0.5 to 0.9 m smaller, because it measures the cube's near face and Valheim measures its centre.

| What you see | What is wrong |
| --- | --- |
| `world.png` has a sky-coloured or black opaque background | The sky or clear-colour hook did not take effect |
| `overlay.png` is solid black | The vignette hook did not take effect |
| Pictures are upside down | Row order |
| Blocks in `world.png` are in a different place than on the Valheim screen | You moved while pressing F9, or the Minecraft window is not the same shape as Valheim's |
| Depth check is off by more than a metre | Depth conversion |
| Red "Frame export failed" line in Minecraft chat | Send the line. Export stays off until Valheim reconnects. |
| Log says Minecraft has not written a frame yet | Minecraft is not connected, has no world loaded, or is minimised |

The frame is as big as the Minecraft window, up to 2560x1440. Making Minecraft render at exactly Valheim's size is part of milestone 4. The F9 key is `[Test] SaveFrameKey` in the plugin's config.

**Passed?** Milestone 4 draws the picture inside Valheim.

### If it does not work

| What you see | Likely cause |
| --- | --- |
| No chat line at all | Look in `BepInEx/LogOutput.log` for a McBridge line ending in `Listening on 127.0.0.1:38517`. If there is no McBridge line, the DLL is in the wrong BepInEx folder (see `Paths.props`). |
| `Could not open 127.0.0.1:38517` in the BepInEx log | Something else has the port, usually a second copy of Valheim. |
| Chat says protocol mismatch | One of the two mods is an old build. Rebuild both. |
| Chat says to switch to creative | The player has to hover, which needs creative mode. Nothing else runs until it is. |
| Chat says this is not a Superflat world | The mod left the world alone. Make a new one with the preset The Void. |
| Minecraft crashes and the log mentions a class ending in `Mixin` | A mixin target did not match. Send the crash report. |
| BepInEx log says it could not create the shared frame file | Send the line. Everything except frame export still works. |
| F8 does nothing in Valheim | Another mod uses F8. Change `PlaceMarkerKey`. |
| Minecraft refuses to start, mentions `mcbridge` | The instance is not 1.21.1, or Fabric API is missing. |

## Bridge protocol, version 3

The Valheim plugin listens on `127.0.0.1:38517`. The Minecraft mod connects and retries every second. Each message is a 4-byte big-endian length followed by that many bytes of UTF-8 JSON. Every message has a `type`.

| `type` | Direction | When | Fields |
| --- | --- | --- | --- |
| `hello` | Both | On connect | `protocol`, `game`, `version`, and from Valheim `engine` (Unity version) and `frames` (path of the shared frame file) |
| `camera` | Valheim to Minecraft | Every rendered frame | `frame`, `x`, `y`, `z`, `yaw`, `pitch`, `fov`, `w`, `h` |
| `marker` | Valheim to Minecraft | On F8, and again when Minecraft reconnects | `x`, `y`, `z`: centre of the white test cube |

Decisions made here that the handoff left open:

- **Valheim sends its own values, unconverted.** Positions are in Unity's left-handed space, yaw is 0 at +Z and clockwise, pitch is positive looking down. The Minecraft side does all the converting, in one file: `Coords.java`.
- **`camera` is sent from Unity's `Camera.onPreCull`,** which runs after every script has moved the camera and just before the frame is drawn. Sending from `Update` would be one frame stale.
- **The shared grid is whole metres.** A 1 m cube centred on a half-integer point in Valheim, such as (12.5, 30.5, -7.5), fills exactly one Minecraft block. This holds for negative coordinates too, so neither side needs special cases.
- **The Minecraft player rides along with the camera.** Chunks load around the player, not the camera, so each tick the player is moved to the camera position, flying, with no velocity. Jumps over 16 m (first connect, Valheim portals) go through a server teleport.

The mapping, which is the handoff's table as implemented and tested:

| Quantity | Valheim | Minecraft |
| --- | --- | --- |
| Position | (x, y, z) | (x, y + `Y_OFFSET`, -z) |
| Yaw | yaw | yaw + 180, wrapped to -180..180 |
| Pitch | pitch | pitch, clamped to -90..90 |
| Vertical FOV | fov | fov |
| Block containing a point | (x, y, z) | (floor(x), floor(y + `Y_OFFSET`), floor(-z)) |

`Y_OFFSET` is 0. Valheim's sea level is y = 30, which is well inside Minecraft's -64 to 319.

### Frame file

Valheim creates one file in the temp folder each time it starts (`mcbridge-frames-*.bin`, 44 MB, deleted when Valheim exits), memory-maps it, and sends its path in `hello`. Minecraft maps the same file and writes every frame into it. Little-endian. Rows run bottom to top.

| Offset | Type | Contents |
| --- | --- | --- |
| 0 | int | Magic `MCBF` |
| 4 | int | Sequence: odd while a frame is being written, even when complete |
| 8, 12 | int, int | Width, height |
| 16 | int | Frame id of the `camera` message the picture was rendered from |
| 64 | RGBA8 | World colour: blocks and particles, premultiplied alpha, transparent where nothing was drawn |
| 64 + 4wh | float32 | World depth: metres along the view axis, 1e30 where nothing was drawn |
| 64 + 8wh | RGBA8 | Overlay: HUD, chat and screens, premultiplied alpha |

A reader notes the sequence, copies what it wants, and reads the sequence again. If it was odd or has changed, the copy is thrown away and tried again.

Decisions made here:

- **Valheim owns the file,** so it can mark it temporary and delete-on-close, which Java cannot. The name changes every run because Minecraft may still have the last one mapped.
- **Depth is converted to metres on the Minecraft side,** using the near and far planes read back out of the projection matrix that drew the frame. Valheim never needs to know Minecraft's projection.
- **Depth is along the view axis,** not straight-line distance. That is what Unity's `LinearEyeDepth` gives for Valheim's side in milestone 5, so the two compare directly.
- **Reads go through pixel buffer objects,** two sets used alternately: this frame's reads are queued and last frame's are copied out. That adds one Minecraft frame of delay and avoids stalling the renderer.
- **The first-person hand is off while exporting.** Valheim is third-person, and drawing the hand wipes the world's depth buffer.

The port and protocol number are defined twice and must match: `Protocol` in `valheim-plugin/Messages.cs` and `dev.mcbridge.protocol.Protocol` in the Minecraft mod. Bump the protocol number whenever a message is added or its fields change. Each side warns on connect if they differ.

## How the code is laid out

Both sides keep the socket code free of game types, so it can be tested without launching either game.

| File | Job |
| --- | --- |
| `valheim-plugin/BridgeServer.cs` | The socket. Background threads for accept, read and write. The game thread never blocks on it, and if Minecraft stops reading, messages are dropped instead of freezing Valheim. |
| `valheim-plugin/Messages.cs` | Message shapes and the protocol constants. |
| `valheim-plugin/Plugin.cs` | The BepInEx plugin: starts the server, sends `camera`, handles incoming messages on the game thread. |
| `valheim-plugin/TestMarker.cs` | The four test cubes for milestone 2. |
| `valheim-plugin/FrameFile.cs` | Creates the shared frame file and copies frames out of it. No Unity types. |
| `valheim-plugin/FramePngs.cs` | The F9 frame dump for milestone 3. |
| `minecraft-mod/src/main/.../net/BridgeClient.java` | The socket, with the retry loop. |
| `minecraft-mod/src/main/.../protocol/` | Message shapes, the protocol constants, `Coords.java` (the only place that converts between the two games' spaces) and `FrameLayout.java` (the byte layout of the frame file and the depth conversion). |
| `minecraft-mod/src/client/.../McBridgeClient.java` | The Fabric client entry point: starts the client, keeps the newest camera pose, moves the player along with it, places the test marker. |
| `minecraft-mod/src/client/.../FrameExporter.java` | Reads the world, its depth and the HUD back from the GPU each frame and writes them to the frame file. |
| `minecraft-mod/src/client/.../mixin/` | `CameraMixin` overrides the camera on every rendered frame. `GameRendererMixin` overrides the field of view and tells the exporter when the world and the HUD have been drawn. `LevelRendererMixin`, `FogRendererMixin` and `GuiMixin` remove sky, clouds, fog and the vignette from exported frames. |
