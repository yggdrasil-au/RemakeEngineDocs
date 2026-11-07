# Terminal User Interface (TUI)

The Remake Engine Terminal User Interface (`EngineNet.Interface.Terminal.TUI`) provides an interactive, keyboard-driven terminal console powered by **Terminal.Gui** and **Spectre.Console**. It is ideal for local developer sessions, interactive operation selection, live log observation, and prompt responses without launching a full graphical window.

Launch the TUI by passing the `--tui` flag:

```pwsh
dotnet run -c Release --project EngineNet --framework net10.0 -- --tui
```

---

## Interactive Menu & Category Views

At startup, the TUI queries the module registry (`Main.json`) and scans local game directories (`EngineApps/Games/`). Game modules are organized under distinct category headers:

```text
Remake Engine — Interactive Terminal Interface
Select a game:
=== [Demo] ===
  demo  [registered, installed, built]
=== [SimpsonsGames] ===
  TheSimpsonsGame-PS3  [registered, installed, unbuilt]
  SimpsonsHitAndRun  [registered, installed, unbuilt]
  TheSimpsonsRoadRage-PS2  [registered, installed, unbuilt]
=== [Lego] ===
  Lego  [registered, installed, unbuilt]
  Lego_Harry_Potter  [registered, installed, unbuilt]
---------------
=== [Internal] ===
  gitDownload
Exit
```

### Visual Styling & Grouping Rules

- **Category Headers (`=== [Category] ===`)**: Rendered in cyan (`ConsoleColor.DarkCyan`). Category headers act as non-selectable visual dividers (mapped internally to `"---"`).
- **Divider (`---------------`)**: Visually separates registered game modules from internal engine tasks.
- **Internal Actions (`=== [Internal] ===`)**: Dedicated section for internal engine utilities discovered from `EngineApps/Registries/ops/` (such as `gitDownload` for cloning remote modules or Git repositories).
- **Exit**: Cleanly terminates the interactive TUI session.

---

## Compound Module State Badges

Beside each module entry, the TUI renders a dynamic state badge evaluated by `GameModuleInfo.DescribeState()`:

| State Descriptor | Status Meaning |
| :--- | :--- |
| **`[registered, installed, built]`** | Module is indexed in `Main.json`, exists on disk with a valid `operations.toml`, and has a verified executable entry point declared in `game.toml`. Ready to play immediately. |
| **`[registered, installed, unbuilt]`** | Module is indexed in `Main.json` and local manifest exists on disk, but required binaries have not yet been produced by build operations. |
| **`[registered, uninstalled]`** | Module is cataloged in `Main.json`, but has not yet been downloaded or cloned to local storage. |
| **`[unverified, installed, built]`** | Local game module discovered on disk that is not listed in `Main.json`, but contains valid operation and game manifests. |
| **`[internal]`** | System task or internal operation. |

Selecting an unbuilt module opens its operation menu, allowing you to run individual steps or execute the full build sequence. Selecting a built module presents the option to launch the game or re-run build operations.

---

## Split-Screen Architecture (`TuiRenderer`)

During operation execution, the TUI switches to a split-screen layout managed by `TuiRenderer`:

```text
┌────────────────────────────────────────────────────────────────────────┐
│ Log Stream                                                      ▲      │
│ [14:20:01] Initializing operations context...                   │      │
│ [14:20:02] Loading Tools.toml dependencies...                   │      │
│ [14:20:03] Probing optical disc: PS3_GAME/USRDIR/EBOOT.BIN      │      │
│ [14:20:05] Extracting assets: 42% (1,240 / 2,950 files)         ▼      │
├────────────────────────────────────────────────────────────────────────┤
│ Status & Progress                                                      │
│ Extraction: [████████████████░░░░░░░░░░░░░░░░░░░░] 42%                 │
│ Target: A:\RemakeEngine\Main\EngineApps\Games\TheSimpsonsGame-PS3\TMP  │
└────────────────────────────────────────────────────────────────────────┘
```

### Viewport Design

- **Upper Viewport (Log Stream)**:
  - Thread-safe scrollable buffer supporting up to 10,000 log entries.
  - Automatically captures stdout and stderr from running scripts, tool processes, and MoonSharp print statements.
  - Box-drawn border using Unicode border glyphs (`┌`, `┐`, `└`, `┘`, `─`, `│`).
- **Lower Viewport (Status & Interactive Prompts)**:
  - Renders active operation status, script-driven progress bars (`PanelProgress`, `ConsoleProgress`), and live elapsed times.
  - Embeds interactive text prompts and confirmation dialogs cleanly without corrupting the scrolling log stream above.
  - Thread-safe cursor management prevents display artifacts and race conditions when background workers output logs.

---

## Keyboard Controls & Process Cancellation

- **Arrow Keys (Up / Down)**: Navigate through menu items and selectable operations.
- **Enter**: Select highlighted item or submit prompt response.
- **Escape / Q**: Return to previous menu level.
- **Ctrl+C Semantics**:
  - **Single Press**: Signals a graceful cancellation token to the currently running step or script.
  - **Triple Press**: Forces an immediate hard process exit (`Environment.Exit(1)`) in case of a hung external tool.
