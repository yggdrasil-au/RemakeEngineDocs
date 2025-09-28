# Initialisation Workflows

Earlier versions of the engine relied on an `init.py` script. The modern EngineNet approach uses manifest metadata instead:

- Mark operations with `init = true` to have them run automatically when a module is selected.
- Combine `init = true` with `Name` to display the operation in logs. Omit `Name` to hide it from manual selection menus.
- Use initialisation steps to validate prerequisites, compute checksums, or populate configuration files before the user triggers other operations.

Example:

```toml
[[bootstrap]]
init = true
script_type = "engine"
script = "validate-files"
args = ["--db", "{{Game_Root}}/config/validation.db", "--base", "{{RemakeEngine.Directories.SourceRoot}}"]
Instructions = "Ensures the source dump contains the expected folder structure."
```

The CLI runs all init operations once per session. The GUI can surface their status in the module dashboard. If an init task fails, the engine reports the error and skips subsequent steps until the failure is resolved.
