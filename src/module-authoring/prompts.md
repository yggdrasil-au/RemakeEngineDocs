# Prompt Authoring

Prompts collect user input before an operation is executed. Prompts are defined under each operation.

## Supported Prompt Types

- confirm: yes or no value
- text: single text value
- checkbox: multi-select values

## Confirm Example

```toml
[[operation.prompts]]
type = "confirm"
Name = "verbose"
message = "Enable verbose output?"
default = false
cli_arg = "--verbose"
```

When true, cli_arg is appended to command arguments.

## Text Example

```toml
[[operation.prompts]]
type = "text"
Name = "source"
message = "Source directory path:"
Required = true
cli_arg_prefix = "--source"
```

The result is appended as two tokens: cli_arg_prefix and input value.

## Checkbox Example

```toml
[[operation.prompts]]
type = "checkbox"
Name = "formats"
message = "Select export formats"
choices = ["glb", "fbx", "obj"]
default = ["glb"]
cli_prefix = "--export"
```

Each selected value is appended after cli_prefix.

## Prompt Rules

- Name values must be unique inside one operation.
- condition values refer to another prompt Name.
- Keep defaults type-consistent with prompt type.
- Prefer clear user messages over short internal abbreviations.
