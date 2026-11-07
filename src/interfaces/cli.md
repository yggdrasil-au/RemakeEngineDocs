# Command-Line Interface (CLI) Reference

The Remake Engine Command-Line Interface (`EngineNet.Interface.Terminal.CLI`) is designed for developer workflows, headless continuous integration (CI) pipelines, batch processing, and ad-hoc script execution.

All commands run through the host executable:

```pwsh
dotnet run -c Release --project EngineNet --framework net10.0 -- [options]
```

---

## Discovery Commands

Discovery commands inspect the engine environment, registered modules, and available manifest operations without modifying disk state:

```pwsh
# List all discovered game modules grouped by category with state badges
dotnet run -c Release --project EngineNet --framework net10.0 -- --list-games

# List internal engine utility modules (e.g., gitDownload) from EngineApps/Registries/ops/
dotnet run -c Release --project EngineNet --framework net10.0 -- --list-internal

# List operations declared in a module's manifest (by module name or ID)
dotnet run -c Release --project EngineNet --framework net10.0 -- --list-ops "TheSimpsonsGame-PS3"
dotnet run -c Release --project EngineNet --framework net10.0 -- --list-ops "demo"

# Print engine version, dynamic build string, and Git commit hash
dotnet run -c Release --project EngineNet --framework net10.0 -- --version

# Print CLI usage and option reference
dotnet run -c Release --project EngineNet --framework net10.0 -- --help
```

### Categorized Module Listing Output

When executing `--list-games`, modules are grouped under their registry categories and accompanied by compound state badges from `DescribeState()`:

```text
[SimpsonsGames]
  - TheSimpsonsGame-PS3  (state: registered, installed, unbuilt; root: A:\RemakeEngine\Main\EngineApps\Games\TheSimpsonsGame-PS3)
  - SimpsonsHitAndRun  (state: registered, installed, unbuilt; root: A:\RemakeEngine\Main\EngineApps\Games\SimpsonsHitAndRun)
  - TheSimpsonsRoadRage-PS2  (state: registered, installed, unbuilt; root: A:\RemakeEngine\Main\EngineApps\Games\SimpsonsGames\TheSimpsonsRoadRage-PS2)
[Lego]
  - Lego  (state: registered, installed, unbuilt; root: A:\RemakeEngine\Main\EngineApps\Games\Lego)
  - Lego_Harry_Potter  (state: registered, installed, unbuilt; root: A:\RemakeEngine\Main\EngineApps\Games\Lego\Games\Lego_Harry_Potter)
[Demo]
  - demo  (state: registered, installed, built; root: A:\RemakeEngine\Main\EngineApps\Games\demo)
```

---

## Execution Modes

The engine provides three primary execution modes:

### 1. Run Predefined Operation (`--run_op`)
Executes a single operation declared in the target module's `operations.toml` or `operations.json`. You can target the operation by numeric ID or by its exact display name:

```pwsh
# Execute by numeric operation ID
dotnet run -c Release --project EngineNet --framework net10.0 -- \
  --game_module "TheSimpsonsGame-PS3" \
  --run_op 12

# Execute by exact operation display name
dotnet run -c Release --project EngineNet --framework net10.0 -- \
  --game_module "demo" \
  --run_op "Lua Feature Showcase"
```

### 2. Run All Operations (`--run_all`)
Sequentially executes all operations defined in the module manifest according to their dependency order:

```pwsh
dotnet run -c Release --project EngineNet --framework net10.0 -- \
  --game_module "demo" \
  --run_all
```

### 3. Inline Script Execution (`--script`)
Runs an arbitrary script file without requiring manifest registration. The engine dynamically builds a synthetic operation context with full access to engine placeholders and SDK globals:

```pwsh
dotnet run -c Release --project EngineNet --framework net10.0 -- \
  --game_module "demo" \
  --script_type lua \
  --script "{{Game_Root}}/scripts/lua_feature_demo.lua" \
  --args '["--module", "{{Game_Root}}", "--scratch", "{{Game_Root}}/TMP/demo"]'
```

---

## Parameter Overrides and Automation

In headless CI or non-interactive environments, CLI flags allow full override of prompt responses, arguments, and manifest configurations:

| Flag / Option | Format | Description | Example |
| :--- | :--- | :--- | :--- |
| `--game_module`, `--game`, `--module` | `<name\|id\|path>` | Identifies the target module by name, registered ID, or disk path | `--game_module "demo"` |
| `--game_root` | `<dir>` | Explicit directory override for the target game module | `--game_root "./EngineApps/Games/demo"` |
| `--internal` | `<id>` | Targets an internal engine module in `EngineApps/Registries/ops/` | `--internal gitDownload` |
| `--ops_file` | `<path>` | Custom manifest path override | `--ops_file "custom_ops.toml"` |
| `--script_type`, `--type` | `<lua\|js\|py\|engine>` | Interpreter runtime for inline scripts | `--script_type lua` |
| `--arg` | `<value>` | Appends a single positional argument string | `--arg "--verbose"` |
| `--args` | `'["a", "b"]'` | Appends multiple arguments as a JSON array or comma-separated list | `--args '["--input", "disc.iso"]'` |
| `--answer` | `KEY=VALUE` | Pre-supplies an answer to an `operations.toml` manifest prompt | `--answer url="TheSimpsonsGame-PS2"` |
| `--auto_prompt` | `KEY=VALUE` | Pre-supplies an answer to an in-script `prompt()` API call | `--auto_prompt build_type=Release` |
| `--set` | `KEY=VALUE` | Injects or overrides raw fields in the operation dictionary | `--set format=iso` |

### PowerShell Quoting Note for `--args`
When passing JSON arrays through PowerShell or cmd.exe, inner double quotes will be stripped unless protected. Always wrap the entire JSON array in **outer single quotes**:

```pwsh
# Recommended PowerShell pattern:
--args '["--module", "{{Game_Root}}", "--scratch", "{{Game_Root}}/TMP/demo"]'
```

### Root CLI vs Script Annotations (`--note`)
The root CLI argument parser (`CLI.cs`) inspects and validates structural engine flags (`--game_module`, `--script`, `--run_op`, `--args`, `--answer`, `--auto_prompt`, `--set`). Script-level annotations or operation tags such as `--note` should be placed inside `--args` or set via `--set`:

```pwsh
# Pass annotation via --args:
--args '["--note", "Nightly regression build"]'

# Or inject via --set:
--set note="Nightly regression build"
```

---

## Built-In Disc Image Operations

The engine includes built-in CLI operation actions for optical media (`script_type = "engine"`), implemented via `DiscImages.IsoFactory.RunCli`:

### 1. `inspect-iso`
Inspects an optical disc image, detects console platform geometry, and lists directory entries matching an optional glob filter:

```pwsh
dotnet run -c Release --project EngineNet --framework net10.0 -- \
  --game_module "demo" \
  --script_type engine \
  --script inspect-iso \
  --args '["--input", "path/to/game.iso", "--filter", "*.xex"]'
```

### 2. `extract-iso`
Extracts matching files from an optical disc image to a target directory with progress reporting and optional overwrite:

```pwsh
dotnet run -c Release --project EngineNet --framework net10.0 -- \
  --game_module "demo" \
  --script_type engine \
  --script extract-iso \
  --args '["--input", "path/to/game.iso", "--output", "path/to/output", "--filter", "USRDIR/*", "--overwrite"]'
```

### 3. `format-extract` (with `format = "iso"`)
Universal multi-format extraction action routing dynamically to the appropriate format handler:

```pwsh
dotnet run -c Release --project EngineNet --framework net10.0 -- \
  --game_module "demo" \
  --script_type engine \
  --script format-extract \
  --set format=iso \
  --args '["--input", "path/to/game.iso", "--output", "path/to/output"]'
```

---

## Exit Codes

| Exit Code | Meaning |
| :--- | :--- |
| `0` | Success / Operation sequence completed without errors |
| `1` | General failure / Script runtime exception / Unhandled error |
| `2` | Invalid arguments / Module or operation not found |
| `130` | Execution cancelled by user (Ctrl+C) |
