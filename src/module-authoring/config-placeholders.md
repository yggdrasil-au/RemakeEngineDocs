# Config Placeholders

config.toml provides module-specific placeholder values used during operation resolution.

## Structure

The schema expects a placeholders array of maps.

```toml
[[placeholders]]
Region = "US"
SourcePath = "{{Project_Root}}/Data/Source"
UseLosslessAudio = true
```

## Behavior

- Each placeholders table contributes key-value pairs to runtime context.
- Later tables override earlier key definitions.
- Values can be string, boolean, integer, or number.

## Recommended Usage

- Put machine-independent defaults in config.toml.
- Keep user-specific secrets and paths outside committed module files when possible.
- Reference placeholder keys from operations.toml using {{KeyName}} syntax.

## Canonical Reference

Use schemas/config.schema.json for field validation and editor IntelliSense.
