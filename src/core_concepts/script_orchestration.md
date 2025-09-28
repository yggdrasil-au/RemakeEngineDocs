# Script & Tool Orchestration

Each operation specifies how the engine should execute work. The `script_type` field dictates the execution path:

- `lua` ? Runs a Lua script through MoonSharp. The engine exposes globals such as `tool`, `argv`, `emit`, `prompt`, and `progress`, plus a rich `sdk` table for filesystem and HTTP helpers.
- `js` ? Executes JavaScript via the embedded Jint engine with parity to the Lua helpers.
- `python` (default) ? Launches an external process via `ProcessRunner`. Python support remains available when you need to call external interpreters or third-party tooling.
- `engine` ? Invokes a built-in handler implemented in C#. Current actions include `download_tools`, `format-extract`, `format-convert`, `validate-files`, `rename-folders`, and `flatten`.
- Any other value ? Treated as an executable name. The engine resolves arguments via placeholders and passes them to `ProcessRunner` unchanged.

During execution, the engine streams stdout/stderr, parses structured events, and refreshes `project.json` so subsequent operations see updated configuration.

Scripts should use `EngineSdk` helpers to emit progress, prompts, and structured logs. Tool paths can be resolved by calling the injected `tool(name)` global, which uses the active `IToolResolver` (JSON manifest, environment override, or PATH fallback).
