# Running the Engine

EngineNet ships with two entry points: the default console project and the Avalonia desktop UI. Both live under the `EngineNet` solution folder.

## Default entry point
```pwsh
dotnet run --project EngineNet
```
If no CLI flags are supplied the engine launches the GUI. Pass `--gui` or `--cli` after the `--` separator to choose explicitly.

```pwsh
dotnet run --project EngineNet -- --gui    # Avalonia interface
dotnet run --project EngineNet -- --cli    # Interactive CLI
```

## Developer CLI examples
Module authors often prefer direct command invocation:

```pwsh
# Run a single operation defined in a manifest
dotnet run --project EngineNet -- \
    --game_module "RemakeRegistry/Games/demo" \
    --script_type engine \
    --script rename-folders
```

Any arguments accepted by the CLI can also be supplied to published binaries or packaged deployments.
