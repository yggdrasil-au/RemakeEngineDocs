# Placeholder System

Placeholders allow manifests and scripts to reference dynamic values without hardcoding paths. They use double braces, e.g. `{{Game.RootPath}}`.

## Sources of placeholder data
1. **Engine config (`project.json`):** All keys are merged into the runtime context using case-insensitive lookup.
2. **Module metadata (`config.toml`):** Values from `[[placeholders]]` tables are merged after engine config keys.
3. **Built-in values:** Injected automatically for convenience:
   - `{{Game_Root}}` and `{{Game.RootPath}}` ? absolute path to the active module.
   - `{{Game.Name}}` ? canonical module identifier.
   - `{{Project_Root}}` ? repository root.
   - `{{Registry_Root}}` ? `RemakeRegistry/` directory.
4. **Prompt answers:** Each prompt stores its answer in the context under its `Name` field.

## Resolution rules
- Dictionaries are traversed via dotted paths (`{{RemakeEngine.Directories.SourceRoot}}`).
- Lists and dictionaries are materialised into JSON-like structures when passed to scripts.
- Strings containing unmatched placeholders remain unchanged, which helps reveal configuration issues.

Use the placeholder system to keep manifests portable?different contributors can adapt `project.json` or `config.toml` without patching every operation.
