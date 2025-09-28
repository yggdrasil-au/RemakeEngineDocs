# Interactive CLI

The CLI offers a menu-driven interface suited to module authors who want fine-grained control and live output streaming.

## Launching
```pwsh
dotnet run --project EngineNet -- --cli
```

## Main menu flow
1. **Game selection** - choose from modules discovered in `RemakeRegistry/Games/`, download new ones from the shared registry, or clone directly from a Git URL.
2. **Initialisation operations** - steps marked with `init = true` run automatically (unless skipped) to prepare the workspace.
3. **Operation menu** - pick individual tasks, run all eligible steps, launch the game, or switch to another module.

## Prompts
- Prompt definitions can include defaults, conditions, and CLI argument mapping. The CLI highlights defaults and stores answers for reuse within the current run.
- Checkbox prompts accept comma-separated values; confirm prompts accept `y`/`n` responses.

## Output
- Standard output from scripts streams in real time with colour hints for stdout/stderr.
- Structured events emitted via `EngineSdk` render colour-coded messages, progress updates, and prompt requests (see [Reading Output & Logs](understanding_output.md)).

Exit the CLI at any time with `Ctrl+C`. Active processes receive a cancellation signal and the engine attempts to terminate them gracefully.
