# Defining Operations

When crafting operations consider the following workflow:

1. **Choose the execution mode** (`script_type`). Prefer `engine` for built-in helpers, `lua` or `js` for inline scripts, and reserve `python` for legacy code.
2. **Reference tools via placeholders**. Always resolve executable paths with `tool("name")` inside scripts instead of hardcoding file system locations.
3. **Collect user input** through prompts where flexibility is needed. Defaults should favour safe operations (e.g., `force = false`).
4. **Chain operations** by writing outputs to predictable locations. Later steps can reference them with placeholders or by reading config updates.
5. **Surface intent** using the `Name` and `Instructions` fields. Clear messaging shortens onboarding for new contributors.

Example TOML snippet:

```toml
[[prepare]]
Name = "Validate source data"
script_type = "engine"
script = "validate-files"
args = ["--db", "{{Game_Root}}/config/checklist.db", "--base", "{{RemakeEngine.Directories.SourceRoot}}"]

[[transform]]
Name = "Convert audio"
script_type = "lua"
script = "Scripts/audio_convert.lua"
argv = ["--input", "{{Game_Root}}/RawAudio", "--output", "{{Game_Root}}/Converted"]
```

Group names (`prepare`, `transform`) help you organise long manifests. The CLI displays them as headings; the GUI can map them to tabs or progress groups.
