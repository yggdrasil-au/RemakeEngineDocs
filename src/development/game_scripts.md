# Writing Game-Specific Scripts

Scripts executed by operations should be small and focused. Use the language you prefer (Python, Lua, etc.). Exit with non‑zero status on failure so the engine can report errors.

Scripts can rely on placeholders expanded by the engine, allowing paths and options to be passed in dynamically.

## Lua Scripts

When the script file ends with `.lua`, it runs inside the embedded MoonSharp interpreter. The engine injects the following helpers:

- `tool(name)` – resolve a registered tool to its absolute path
- `argv` – string array of arguments passed to the script
- `emit(event, data?)` – emit a structured engine event
- `warn(message)` / `error(message)` – convenience logging
- `prompt(message, id?, secret?) -> string` – interactive prompt
- `progress(total, id?, label?) -> handle` – create a progress handle; call `handle:Update(inc?)`
- `sdk.*` – utility helpers for config and filesystem:
  - `sdk.ensure_project_config(root) -> string` – ensure `project.json` exists; returns its path
  - `sdk.validate_source_dir(path) -> bool` – check that a directory exists and is accessible
  - `sdk.copy_dir(src, dst, overwrite?) -> bool` – recursive copy
  - `sdk.move_dir(src, dst, overwrite?) -> bool` – move (cross‑drive safe)

Example:

```lua
-- Resolve tools and parse args
local src = argv[1]
local out = argv[2]

-- Ensure a project config at repo root
local config_path = sdk.ensure_project_config(".")
warn("Using config: " .. config_path)

-- Validate the source directory
if not sdk.validate_source_dir(src) then
  error("Invalid source directory: " .. tostring(src))
  return 1
end

-- Show progress while copying
local p = progress(100, "copy", "Copying files")
emit("info", { step = "copy", src = src, dst = out })
local ok = sdk.copy_dir(src, out, true)
p:Update(100)
if not ok then
  error("Copy failed")
  return 1
end

warn("Done!")
return 0
```
