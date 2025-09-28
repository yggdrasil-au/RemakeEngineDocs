# Writing Script Actions

## Lua
- Place scripts under `Scripts/` and reference them with `script_type = "lua"`.
- Globals include `tool(name)`, `argv`, `emit`, `warn`, `error`, `prompt`, and `progress`.
- The `sdk` table exposes filesystem helpers (`ensure_dir`, `copy_dir`, `move_dir`, `remove_file`, `create_symlink`, `realpath`, `run_process`, etc.), configuration utilities (`ensure_project_config`, `validate_source_dir`), TOML read/write shims, and hashing helpers such as `md5`.
- Use the `sqlite` module (`sqlite.open`, `handle:exec`, `handle:query`, transactions) for lightweight database work without leaving the process.

## JavaScript
- Use `script_type = "js"` to run scripts inside the Jint engine.
- The helper surface mirrors Lua: `tool`, `argv`, `emit`, `warn`, `error`, `prompt`, `progress`, plus an `sdk` object and `sqlite` module with the same capabilities.
- `sdk.run_process([...])` captures stdout/stderr and supports timeouts and environment overrides when chaining other tools.

## External executables
- For Python or third-party binaries set `script_type` to the executable name (for example `python`, `quickbms`).
- Provide arguments via the manifest `args` array and rely on placeholders to inject per-user paths or prompt answers.

### Best practices
- Emit structured events (`EngineSdk.Emit`) or `EngineSdk.Print` for messages so the GUI and CLI stay in sync.
- Fail fast with descriptive error messages; non-zero exit codes are surfaced as operation failures.
- Keep scripts idempotent where possible. Users often rerun operations while iterating on manifests.
- Resolve tool paths with `tool("id")` instead of hardcoding binaries; this respects `Tools.local.json`, `Tools.json`, and environment overrides.
