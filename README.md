# cinemascope
Native Horizon mod for Minecraft 26.3 that exports Studio replays to H.264 MP4 with an optional 2.39:1 cinemascope crop, replay timeline HUD, and camera z

**Cinemascope** (`cinemascope.hnmod`) is a native Horizon mod for **Minecraft 26.3** that exports video directly from Horizon Studio replays with an optional **2.39:1 cinemascope crop**.

## How It Works

Horizon is a Rust rewrite of Minecraft whose native mods are source bundles compiled directly into the game by the launcher.

Horizon’s built-in **Studio** already handles `.mcpr` replay playback, free-cam movement, and camera paths, but its native Export button currently does not output a file. Because Horizon's Mod API (`hooks API 1`) cannot inject replay packets into the world or capture frames directly, Cinemascope relies on Studio for playback and handles the export externally:

1. **Playback:** Horizon Studio plays back the `.mcpr` replay in-world.
2. **Capture:** A background thread captures screenshots through `horizon-ctl`.
3. **Encoding:** Frames are piped directly into `ffmpeg` to write an H.264 `.mp4` timed to the real clock, with optional 2.39:1 cropping.

## Prerequisites

* **Horizon Launcher** (Minecraft 26.3)
* **`ffmpeg`** installed and available in your system `PATH`
* **Agent Control** enabled in Horizon settings (required for `horizon-ctl`)

## Installation

1. Build or download `cinemascope.hnmod` (v0.3.0).
2. Place `cinemascope.hnmod` into your Horizon mods folder (replacing any older builds).
3. Launch Horizon and allow the launcher to recompile the client with the mod.

## Features & Usage

| Feature | How to use it |
| :--- | :--- |
| **Video export** | Press `F8`, or run `/replay export [file.mp4] [fps]`, then play your replay in Studio. Press `F8` again or run `/replay stop` to finish. |
| **Scope crop** | Trims the video to **2.39:1**. Toggle it on or off in *Options > Mods > Cinemascope*. |
| **Test export** | Run `/replay exporttest /home/jenrick/test.mp4` to generate a 3-second test pattern and confirm `ffmpeg` works on your system. |
| **Replay file reader** | Run `/replay open <file.mcpr>` to inspect a replay file and display a HUD timeline box showing time, speed, and packet count. Use `K`, `J`, `.`, and `,` to control that clock. |
| **Zoom** | Hold `C` for a **1.6x** camera zoom. |

## Current Status & Limitations

* **Confirmed working:** Building and loading the mod, parsing `.mcpr` files, and generating the `ffmpeg` test-pattern video.
* **In testing (v0.3.0):** Live `horizon-ctl` screenshot export during Studio playback, and `F8` keybind responsiveness while Studio is open.
* **Real-time capture speed:** Because recording runs in real time rather than frame-by-frame, exports typically run at **8–15 FPS** and can be choppy during fast camera moves.
* **Separate HUD clock:** The `/replay open` timeline box runs on its own clock and does not control Studio’s in-world playback.
* **Frame-by-frame rendering:** Smooth deterministic rendering would require Studio’s internal command names, which are currently undocumented.
