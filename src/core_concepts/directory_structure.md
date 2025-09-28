# Project Directory Layout

```
RemakeEngine/
|-- EngineNet/                        # Core engine assemblies and CLI
|-- EngineNet.Interface.GUI.Avalonia/ # Desktop GUI
|-- EngineNet/specs/                  # Specification docs that describe expected behaviour
|-- RemakeRegistry/
|   |-- Games/
|   |   |-- <ModuleName>/            # Operations, scripts, and assets for a specific game
|   |-- Tools.json                    # Central registry of downloadable tools
|-- Tools/                            # Shared tools checked into the repo
|-- RemakeEngineDocs/                 # This documentation set (mdBook)
|-- project.json                      # User-specific configuration (generated on first run)
|-- RemakeEngine.sln                  # Solution file
```

Within a module directory you will typically see:

```
RemakeRegistry/Games/<ModuleName>/
|-- operations.toml / operations.json  # Primary manifest for operations
|-- config.toml                        # Placeholder values merged into the runtime context
|-- Scripts/                           # Lua, JavaScript, or helper assets
|-- Tools/                             # Module-specific binaries (optional)
|-- README.md                          # Module documentation
```

The engine reads these files at runtime; no build step is required when editing manifests or scripts.
