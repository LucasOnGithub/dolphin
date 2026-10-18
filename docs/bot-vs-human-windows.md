# Bot-vs-Human Windows build

This branch has a dedicated GitHub Actions workflow: **Bot-vs-Human Windows x64**.

1. Open the repository **Actions** tab.
2. Select **Bot-vs-Human Windows x64**.
3. Choose **Run workflow** and select **bot-vs-human**.
4. After a successful run, download the **bot-vs-human-windows-x64** artifact from its **Artifacts** section.
5. Extract the archive into a new folder (do not overwrite the official Slippi Launcher install).

The workflow also builds automatically when build inputs change on the bot-vs-human branch.

## Included binaries

- **Slippi_Dolphin.exe** — GUI Slippi netplay build.
- **dolphin-emu-nogui.exe** — NoGUI frontend; choose the Windows headless platform with --platform=headless.
- **slippi_rust_extensions.dll**, the other runtime dependencies, **Sys**, and **portable.txt**.

Example headless launch from PowerShell, using your own paths:

    .\dolphin-emu-nogui.exe --platform=headless -e "A:\roms\Melee.iso" -u "A:\SlippiBot\User"

For completely unattended use, configure that Dolphin user folder with the **Null video backend** and audio disabled. The headless platform selection controls the window system, but does not by itself guarantee null rendering or muted audio.

## Buffer donation / run-ahead

The bot-vs-human branch contains a bot-aware run-ahead implementation in
Source/Core/Core/HW/EXI/EXI_DeviceSlippi.cpp. Building both frontends from the
same branch retains this feature.

**Important:** The logic is active only when the matchmaking response marks the
local player as a bot (is_bot). A special executable name, --platform=headless,
or this CI workflow cannot change server-side classification.

## Troubleshooting

- If the workflow is missing or no runs appear, enable Actions for this fork in its GitHub settings, then open the Actions tab and launch it manually.
- If compilation fails, inspect the **Configure** or **Build** steps in its job logs. The workflow uploads the build only after the required executables and Rust DLL exist.
- A compiled artifact does not automatically prove that online netplay, run-ahead, or headless operation works in a real Windows session; validate those locally.
- Your own legally obtained Melee ISO is required. It is not included in the artifact.
