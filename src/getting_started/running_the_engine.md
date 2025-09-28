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

`Program.cs` automatically discovers the project root by searching for `RemakeRegistry/Games`. Override it with `--root <path>` when running the CLI directly or when working from a manifest checkout.

## CLI commands
```pwsh
dotnet run --project EngineNet -- --list-games          # Show detected modules
dotnet run --project EngineNet -- --list-ops demo        # List operations for a module
dotnet run --project EngineNet -- --game_module demo \
    --script download_tools --script_type engine         # Run a single operation inline
```

Inline invocations accept the same arguments as manifest operations. Supply prompt answers with `--answer Name=value` or bake defaults into the manifest. Published binaries expose the same interface, so these commands work once the project is installed.
