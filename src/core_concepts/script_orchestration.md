# Script & Tool Orchestration

Each operation describes how EngineNet should execute work. The `script_type` field selects the runtime path:

- `lua` - Runs a Lua script through MoonSharp. The engine injects globals such as `tool`, `argv`, `emit`, `warn`, `error`, `prompt`, and `progress`. A shared `sdk` table provides filesystem helpers (`ensure_dir`, `copy_dir`, `remove_file`, `create_symlink`, `realpath`, `run_process`, etc.), configuration utilities (`ensure_project_config`, `validate_source_dir`), hashing helpers (`md5`), simple TOML read/write shims, and structured printing via `color_print`. Lua scripts also receive a `sqlite` module that exposes `open`, `exec`, `query`, transactions, and automatic resource cleanup.
- `js` - Executes JavaScript via the embedded Jint engine. The helper surface mirrors the Lua runtime: `tool`, `argv`, `emit`, `warn`, `error`, `prompt`, `progress`, an `sdk` object with the same filesystem and process helpers, and a `sqlite` module that returns disposable handles with `exec`, `query`, `transaction`, and `close` methods.
- `python` (default) - Launches an external process via `ProcessRunner`. When `script_type` is omitted the engine assumes `python` (or `python3` on Unix-like systems) and invokes the resolved script path with any provided arguments.
- `engine` - Invokes a built-in handler implemented in C#. These are the current operations:
  - `download_tools` resolves a manifest path (from `tools_manifest` or the first argument) and feeds it to `ToolsDownloader`, honouring prompt answers such as `force_download`.
  - `format-extract` dispatches to either `TxdExtractor` (`format = "txd"`) or `QuickBmsExtractor` (`format = "str"`) after resolving placeholder-expanded arguments.
  - `format-convert` runs the managed `MediaConverter` when `tool` is `ffmpeg` or `vgmstream`, enabling consistent audio conversions across platforms.
  - `validate-files` executes `FileValidator`, which checks that expected directories, files, and checksums exist before continuing.
  - `rename-folders` applies batch rename rules through `FolderRenamer`.
  - `flatten` / `flatten-folder-structure` reorganises directory trees using `DirectoryFlattener`.
- Any other value - Treated as an executable name. The engine resolves arguments through placeholders and starts the process via `ProcessRunner` without further intervention.

The command builder merges `project.json`, `config.toml`, built-in placeholders (`Game_Root`, `Project_Root`, `Registry_Root`, `Game.Name`, etc.), and prompt answers before resolving paths. After every operation the engine reloads `project.json` so subsequent steps see changes written by scripts or built-in actions.
