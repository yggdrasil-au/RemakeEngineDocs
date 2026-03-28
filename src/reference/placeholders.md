# Placeholders

Placeholders are string tokens in the form {{KeyName}} resolved by runtime context before operation execution.

## Common Built-In Placeholders

- {{Game_Root}}: absolute module root path
- {{Project_Root}}: absolute repository root path

## Module Placeholders

Module-specific placeholder keys come from config.toml placeholders tables.

## Resolution Scope

Placeholder expansion applies to operation script paths, arguments, and nested operation payload values.

## Resolution Timing

- Parent operation values are resolved before parent execution.
- onsuccess child operations resolve using latest runtime context at child execution time.

## Good Practices

- Use descriptive placeholder names.
- Keep placeholders stable across operation revisions.
- Avoid hardcoding machine-specific absolute paths in operation files.
