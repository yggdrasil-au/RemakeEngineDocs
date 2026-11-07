# Avalonia Desktop GUI Reference

The Remake Engine Graphical User Interface (`EngineNet.Interface.GUI`) is a modern desktop application built on **Avalonia UI 12.1** and **ReactiveUI**. It delivers a unified, visual management studio for browsing game modules, downloading repositories from the central registry, triggering operation pipelines, inspecting real-time build logs, and launching rebuilt games.

The GUI launches automatically when running the engine without arguments, or explicitly with the `--gui` flag:

```pwsh
# Launch GUI explicitly
dotnet run -c Release --project EngineNet --framework net10.0 -- --gui

# Or simply run without CLI parameters (default startup mode)
dotnet run -c Release --project EngineNet --framework net10.0
```

---

## Desktop Architecture & Platform Support

Built as a cross-platform desktop client, `EngineNet.Interface.GUI` runs natively across:
- **Windows (x64 / ARM64)**: DirectX, DirectWrite, and Win32 integration with automatic console window detachment (`ConsoleHelper.FreeConsole`).
- **Linux (x64 / ARM64)**: X11 and Wayland rendering.
- **macOS (Apple Silicon / Intel)**: Cocoa and Metal desktop backend.

The user interface follows MVVM patterns using ReactiveUI, bound to core engine services through `MiniEngineFace` and `OperationOutputService`.

---

## Page Overview & Workflows

Navigation is managed via the top navigation bar in `MainWindow.axaml`, providing access to five dedicated pages:

```text
┌────────────────────────────────────────────────────────────────────────┐
│  [Remake Engine]    Library    Store    Module    Building    Settings │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│  Active Page Content                                                   │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

### 1. Library Page (`LibraryPage.axaml`)

The Library is the central game hub displaying installed and discovered modules on the local workstation:
- **Category Grouping & Ordering**: Modules are organized and sorted by `Category` (e.g. `Demo`, `SimpsonsGames`, `Lego`) and then by `ModuleName`.
- **Cover Art Resolution**: The engine dynamically inspects each module's root directory (`ResolveCoverUri`) to locate and display cover banners, game posters, or fallback icons.
- **Category Badge**: Every card features a blue category pill (`#2563EB`) indicating its parent category.
- **State Badges**: Clearly shows module lifecycle status: `[Built]`, `[Unbuilt]`, `[Registered]`, or `[Unverified]`.
- **Dynamic Primary Action Button**:
  - If `IsBuilt == true`: Displays **"Play"**, directly launching the executable through `GameLauncher`.
  - If `IsBuilt == false`: Displays **"Run All Build Operations"**, triggering the full module build pipeline.
- **Details Button**: Navigates directly to the Module Page for granular step inspection.

### 2. Store Page (`StorePage.axaml`)

The Store serves as the package and catalog manager connected to the central module registry (`EngineApps/Registries/Modules/Main.json`):
- **Catalog Browsing**: Loads registered modules from the official online repository or local registry cache.
- **Live Search & Filtering**: Filters modules instantly across module names, display titles, and category tags.
- **One-Click Download & Installation**:
  - Displays **"Download"** for uninstalled modules, triggering `GitTools.CloneModule` with automatic `--recurse-submodules` cloning into the configured path.
  - Displays **"Install / Open"** for downloaded modules.
- **Status Badges**: Displays current local status (`Downloaded`, `Not Downloaded`) and category pills.

### 3. Module Page (`ModulePage.axaml`)

A single-module operations console providing granular inspection and step execution:
- **Header Metadata**: Displays module title, filesystem root path (`GameRoot`), executable target path, Git repository URL, and registry ID.
- **Module Actions**:
  - **Play**: Launches the game if built.
  - **Run All**: Executes all manifest operations in dependency sequence.
  - **Stop**: Signals cancellation to running operations.
  - **Download**: Clones or updates remote assets.
  - **Open Folder**: Opens the module directory in the host file manager.
- **Interactive Operations Table**: Lists all operations defined in `operations.toml` (ID, Display Name, Description, Group/Phase).
- **Individual Step Execution**: Every row features a dedicated **"Run"** button to execute that specific step in isolation, ideal for testing and debugging individual script phases.

### 4. Building Page (`BuildPage.axaml`)

The Building Page provides a live diagnostics console during operation execution:
- **Live Output Streaming**: Binds directly to `OperationOutputService.Instance`, streaming real-time stdout and stderr output from scripts, external tools, and compilers.
- **Auto-Scroll Pinning**: Automatically keeps the latest log lines in view, but seamlessly unpins scrolling when the user scrolls up to inspect previous output.
- **Log Management**: Includes controls to pause auto-scrolling, copy log selections, and clear the output view.

### 5. Settings Page (`SettingsPage.axaml`)

The Settings view provides global environment configuration:
- **Project Root**: Displays and configures the active engine workspace root (`EngineNet.Shared.State.RootPath`).
- **Tool Paths**: Displays directories scanned for external tools and compiler dependencies (`EngineApps/Tools/`).

---

## Interactive Modal Dialogs & Prompts

When operations require user choices or input, the GUI displays non-blocking dialogs:

- **`TextPromptWindow`**: Text entry modal for string inputs, paths, or custom configuration parameters.
- **`ConfirmWindow`**: Yes/No/Cancel confirmation dialog for destructive actions or branch selections.
- **In-Window Prompt Overlay**: Embedded overlay banner in `MainWindow` offering direct buttons (`Yes`, `No`, `Submit`, `Cancel`), ensuring execution flows smoothly without window management clutter.
