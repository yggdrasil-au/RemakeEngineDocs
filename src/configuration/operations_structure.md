# Structure and Required Fields

An operation object supports the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `Name` | string | Display label in the CLI/GUI. Optional for hidden init tasks. |
| `script_type` | string | Execution mode (`lua`, `js`, `engine`, `python`, or a custom executable). Defaults to `python`. |
| `script` | string | Path to the script or identifier of the engine action. Required unless `script_type` resolves to an executable that does not need a script (e.g., `engine` operations may still use this to name the action). |
| `args` | array | Positional arguments passed to the script or executable. Placeholders are resolved before execution. |
| `env` | object | Additional environment variables merged into the subprocess environment. |
| `prompts` | array | Prompt definitions consumed before execution. See [Prompts & User Input](../core_concepts/advanced_input.md). |
| `init` | bool | When `true`, the operation runs automatically when the module loads. Typically used for validation tasks. |
| `run-all` | bool | Hint to include the operation when the user chooses "Run All". |
| `Instructions` / `description` | string | Optional human-readable summary. |

Additional custom fields can be included for scripts to read from the operation dictionary. Scripts receive the full object, so you can extend the schema in module-specific ways.
