# Engine Entry Points

EngineNet exposes two managed entry points:

- **`EngineNet/Program.cs`** ? Console entry point that selects between GUI and CLI modes.
- **`EngineNet.Interface.GUI.Avalonia/`** ? Desktop application hosting the Avalonia UI.

When building modules:
- Keep init logic in manifest operations (`init = true`) or dedicated scripts executed via `script_type`.
- Use the CLI or GUI entry points above to test workflows end-to-end.
