# Operations Manifest

operations.toml is the main workflow definition for a module. Operations are declared using TOML array-of-table syntax.

## Basic Structure

```toml
[[operation]]
id = 10
Name = "Extract Assets"
script_type = "lua"
script = "{{Game_Root}}/operations/extract.lua"
args = ["--out", "{{Game_Root}}/Extracted"]
run-all = true
init = false
depends-on = [1]
```

## Common Fields

- id: unique integer per operation
- Name: display text used in UI and TUI
- script_type: runtime type such as lua, js, python, bms, or engine
- script: path or operation token interpreted by script_type
- args: argument list with placeholder support
- run-all: include in run-all execution flow
- init: run during initialization and hide from normal menu lists
- depends-on: operation IDs that must succeed first
- prompts: interactive prompt definitions
- onsuccess: child operations executed only after success

## Execution Notes

- Placeholder values are resolved recursively before execution.
- onsuccess operations are resolved at child execution time, using current context.
- dependency graphs are validated by engine runtime before run-all execution.

## Canonical Specification

Use schemas/operations.toml.md and schemas/operations.schema.json as the canonical format references when adding new fields.
