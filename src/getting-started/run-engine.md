# Run the Engine

EngineNet supports three user experiences: GUI, TUI, and direct CLI execution.

## Default Entry Point

```pwsh
dotnet run -c Release --framework net10.0 --project EngineNet
```

When no specific mode flags are provided, the engine chooses its default startup behavior.

## Launch GUI

```pwsh
dotnet run -c Release --framework net10.0 --project EngineNet -- --gui
```

## Launch TUI

```pwsh
dotnet run -c Release --framework net10.0 --project EngineNet -- --tui
```

## Execute a Direct CLI Operation

```pwsh
dotnet run -c Release --framework net10.0 --project EngineNet -- --game_module "EngineApps/Games/demo" --script_type lua --script "{{Game_Root}}/scripts/lua_feature_demo.lua"
```

Use direct CLI execution for automation and ad-hoc script runs.
