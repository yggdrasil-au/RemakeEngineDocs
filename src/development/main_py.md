# Engine Entry Points

The legacy engine relied on `main.py`. EngineNet replaces it with two managed entry points:

- **`EngineNet/Program.cs`** ? Console entry point that selects between GUI and CLI modes.
- **`EngineNet.Interface.GUI.Avalonia/`** ? Desktop application hosting the Avalonia UI.

When porting old modules:
- Move initialisation code into manifest `init` operations or dedicated scripts.
- Replace Python-specific helper calls with Lua/JavaScript equivalents or built-in engine actions.

Retain `LegacyEnginePy/main.py` only for historical reference or when troubleshooting legacy modules.
