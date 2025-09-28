# Interactive CLI

The CLI offers a menu-driven interface suited to module authors who want fine-grained control and live output streaming.

## Launching
```pwsh
dotnet run --project EngineNet -- --cli
```

## Main menu flow
1. **Game selection** ? choose from modules discovered in `RemakeRegistry/Games/`. If none are available, the CLI prompts you to download or create one.
2. **Initialisation operations** ? operations marked with `init = true` run automatically (unless skipped) to prepare the workspace.
3. **Operation menu** ? pick individual tasks, run all eligible steps, or switch to another module.

## Prompts
- Each prompt definition can include defaults, conditions, and CLI argument mapping. The CLI highlights defaults and stores answers for reuse within the current run.
- Checkbox prompts accept comma-separated values; confirm prompts accept `y`/`n` responses.

## Output
- Standard output from scripts is streamed in real time.
- Structured events emitted via `EngineSdk` are rendered with colour-coded messages (see [Reading Output & Logs](understanding_output.md)).

You can exit the CLI at any time with `Ctrl+C`; active processes receive a cancellation signal and the engine attempts to terminate them gracefully.
