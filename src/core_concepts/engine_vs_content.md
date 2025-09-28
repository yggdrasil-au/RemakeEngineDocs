# Engine vs. Content

Remake Engine distinguishes between core functionality and per-game content:

- **Engine (EngineNet):**
  - C# projects under `EngineNet/` and `EngineNet.Interface.GUI.Avalonia/`.
  - Provides configuration loading, placeholder resolution, prompt handling, script execution, and built-in operations.
  - Changes here affect every module and must be accompanied by specification updates in `EngineNet/specs/`.

- **Content (Modules):**
  - Lives under `RemakeRegistry/Games/<ModuleName>/`.
  - Contains operation manifests, module-specific scripts, placeholder TOML files, and documentation.
  - Can be added, removed, or modified without touching engine code.

This separation keeps the engine reusable while allowing each module to describe its own workflows declaratively. When a module needs a new capability, we extend the engine to support it, then capture the behaviour in a spec and manifest.
