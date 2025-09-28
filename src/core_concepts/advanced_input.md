# Prompts & User Input

Operations can collect information at runtime through the `prompts` array. Each prompt object supports the following fields:

- `Name` (string, required) ? Key used to store the answer in the placeholder context.
- `type` (string) ? `text`, `confirm`, or `checkbox`. Defaults to `text`.
- `message` (string) ? Displayed to the user.
- `default` ? Default value (string, boolean, or list depending on `type`).
- `cli_arg` / `cli_arg_prefix` / `cli_prefix` ? Allow mapping answers back into command-line arguments when building scripts.
- `condition` (string) ? Name of another prompt; if that prompt resolves to a truthy boolean the current prompt is shown.

Examples:

```toml
[[prompts]]
Name = "sourceDir"
type = "text"
message = "Where are the original assets?"
default = "{{RemakeEngine.Directories.SourceRoot}}"

[[prompts]]
Name = "force"
type = "confirm"
message = "Re-download tools even if they exist?"
default = false
```

Answers live for the duration of the operation group and can be reused via placeholders (`{{sourceDir}}`, `{{force}}`). The CLI and GUI honour defaults, display conditions, and ensure values are available before any arguments are constructed.
