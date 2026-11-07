# Module Layout & Registry Architecture

A game module is an isolated, self-contained workspace encapsulating the reverse-engineering rules, asset extractions, format conversions, and launch definitions for a specific game remake.

Game modules reside within `EngineApps/Games/` and are indexed in the central registry at `EngineApps/Registries/Modules/Main.json`.

---

## Standard Module Directory Skeleton

A fully equipped game module typically contains the following layout:

```text
EngineApps/Games/<Category>/<ModuleName>/  (or EngineApps/Games/<ModuleName>/)
├── operations.toml       # Primary operation manifest (pipeline steps, scripts, prompts)
├── config.toml           # Module-specific variables and local configuration placeholders
├── game.toml             # Game metadata, launch executable path, and display properties
├── Tools.toml            # External tool requirements (e.g., QuickBMS, FFmpeg, ImageMagick)
├── scripts/              # Module-specific execution scripts (Lua, JavaScript, Python)
├── operations/           # Additional operation scripts or sub-pipeline handlers
└── TMP/                  # Temporary working directory for generated or intermediate data
```

### Manifest Roles

| Manifest File | Format | Schema Reference | Purpose |
| :--- | :--- | :--- | :--- |
| **`operations.toml`** | TOML / JSON | `schemas/operations.schema.json` | Declares operations, steps, parameters, script references, and prompt questions. |
| **`config.toml`** | TOML | `schemas/config.schema.json` | Stores module placeholders resolved at runtime (e.g., game paths, user preferences). |
| **`game.toml`** | TOML | `schemas/game.schema.json` | Defines launch properties, executable targets, and game identity. |
| **`Tools.toml`** | TOML | `schemas/tools.schema.json` | Declares required tool binaries and download URLs. |

---

## Central Module Registry (`Main.json`)

Modules are indexed centrally in `EngineApps/Registries/Modules/Main.json`. The engine dynamically parses both hierarchical category containers and flat legacy entries:

```json
{
  "modules": {
    "Demo": {
      "demo": {
        "id": "480",
        "path": "EngineApps/Games/demo"
      }
    },
    "SimpsonsGames": {
      "TheSimpsonsGame-PS3": {
        "id": "1",
        "path": "EngineApps/Games/TheSimpsonsGame-PS3",
        "url": "https://github.com/Superposition28/TheSimpsonsGame-PS3.git"
      },
      "SimpsonsHitAndRun": {
        "id": "3",
        "path": "EngineApps/Games/SimpsonsGames/SimpsonsHitAndRun",
        "url": "https://github.com/Superposition28/SimpsonsHitAndRun.git"
      }
    },
    "Lego": {
      "Lego": {
        "id": "5",
        "path": "EngineApps/Games/Lego",
        "url": "https://github.com/Superposition28/Lego.git"
      },
      "Lego_Harry_Potter": {
        "id": "6",
        "path": "EngineApps/Games/Lego/Games/Lego_Harry_Potter"
      }
    }
  }
}
```

### Category Containers & Inheritance

1. **Category Containers**: Any JSON object under `"modules"` that is not a module definition (lacks `id`, `path`, or `url`) functions as a Category Container (e.g. `"Demo"`, `"SimpsonsGames"`, `"Lego"`).
2. **Category Inheritance**: Modules defined inside a category container automatically inherit that category name unless an explicit `"category"` property is specified.
3. **Dynamic Relocation Matching**: In Git checkouts, submodules are often cloned into flat directory paths (e.g. `EngineApps/Games/SimpsonsHitAndRun`) even if `Main.json` declares a categorized catalog path (`EngineApps/Games/SimpsonsGames/SimpsonsHitAndRun`). `ModuleScanner` dynamically correlates disk paths with registry metadata, maps the module to its inherited category, and updates runtime paths automatically.

---

## Nested Submodules & Git Hierarchy

Large remake projects often organize games in parent-child hierarchies or include specialized reverse-engineering submodules:

1. **Recursive Git Cloning**: When cloning modules via the Store or CLI, the engine executes:
   ```bash
   git clone <url> <target> --recurse-submodules
   ```
   This ensures that any submodules defined inside the repository's `.gitmodules` (such as `Lego/Games/Lego_Harry_Potter`) are checked out and initialized automatically.
2. **BFS Scanning with Depth Limit**: `Registries.EnumerateModuleDirectories` performs breadth-first scanning up to `maxDepth = 4`.
3. **Continued Traversal into Parent Modules**: When the scanner discovers a valid module (such as `EngineApps/Games/Lego`), it records that module and **continues recursing into child folders**. This allows nested game modules (such as `EngineApps/Games/Lego/Games/Lego_Harry_Potter`) to be discovered and registered.

---

## Directory Pruning Rules (`_prunedDirectoryNames`)

To prevent performance degradation and avoid false module discoveries in large repositories containing non-module submodules (such as documentation, format viewers, or raw game dumps), the directory scanner automatically prunes:

1. **Dot-Directories**: Any directory starting with `.` (e.g. `.git`, `.betterGit`, `.history`, `.vscode`).
2. **Hidden and System Folders**: Evaluated via filesystem attributes `FileAttributes.Hidden | FileAttributes.System`.
3. **Asset & Internal Code Directories (`_prunedDirectoryNames`)**:
   - `Source` (C/C++ reverse engineering source trees)
   - `GameFiles` (Raw dumped game data)
   - `Assets` (Extracted raw art assets)
   - `TMP` (Temporary working files)
   - `operations` (Internal operation scripts)
   - `scripts` (Module scripts)
   - `reversing` (Research notes, Ghidra projects, docs)
   - `research` (Reverse engineering research)
   - `GodotProject` (Godot client project files)
   - `docs` (Documentation files)
   - `tools` (Local tool binaries)
   - `bin` / `obj` (Compilation output)
   - `node_modules` (Web/JS dependencies)

---

## Module State Lifecycle

Module readiness is evaluated dynamically across five state flags:

| State Flag | Qualification Criteria | Description |
| :--- | :--- | :--- |
| **`IsRegistered`** | Module entry exists in `Main.json` | Cataloged in the central registry. |
| **`IsInstalled`** | Directory on disk contains `operations.toml` or `operations.json` | Manifest is present and readable. |
| **`IsBuilt`** | Directory contains `game.toml` with a runnable entry point on disk (`exe`, `lua`, or `godot`) | Binary exists and can be launched immediately. |
| **`IsUnverified`** | `IsInstalled == true`, but not in `Main.json` | Local custom or experimental module. |
| **`IsInternal`** | Discovered from `EngineApps/Registries/ops/*.toml` | Engine system task (e.g. `gitDownload`). |

`GameModuleInfo.DescribeState()` compiles these flags into compound badges (e.g., `[registered, installed, built]`, `[registered, installed, unbuilt]`) shown in TUI, CLI, and GUI.

---

## Game Module Conventions & Best Practices

From game module guidelines (`EngineApps/Games/*/AGENTS.md` and `cli.md`):

1. **Dynamic Tool Path Resolution**:
   External tools installed in `EngineApps/Tools/` use versioned directory names (e.g. `Blender-4.5.4-win-x64`, `ffmpeg-8.0-win-x64`, `Godot-4.6-stable-mono-win-x64`). Modules and scripts must inspect `EngineApps/Tools/` dynamically to resolve the currently installed version rather than hardcoding static paths.
2. **Lua Type Annotations**:
   Module scripts must strictly use EmmyLua type annotations (`---@class`, `---@param`, `---@return`, `---@cast`) to maintain IDE IntelliSense against `EngineApps/api_definitions/remake-engine/`.
3. **Temporary Directories (`TMP/`)**:
   Modules must direct intermediate extractions, temporary dumps, and conversion caches to `{{Game_Root}}/TMP/`. This folder is excluded from version control and automatically pruned by the scanner.
4. **Shell Quoting**:
   When invoking operations with JSON parameters from PowerShell, always wrap arguments in single outer quotes (`--args '["--input", "path"]'`) to prevent PowerShell from stripping inner double quotes.
