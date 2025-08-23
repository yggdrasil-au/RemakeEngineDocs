# Structure and Fields

Each operation object can contain the following keys:

- `Name` – display name in the menu
- `Type` – `script` (default) or `download`
- `script` – path to a Python file when `Type` is `script`
- `args` – list of command-line arguments, supports placeholders
- `init` – run automatically during game selection
- `python_executable` – override interpreter for scripts
- `url`, `destination`, `filename`, `unpack` – fields specific to `download`
- `prompts` – array of prompt objects for runtime input
