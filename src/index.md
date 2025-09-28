# Welcome to the Remake Engine Docs

Remake Engine is a configuration-driven orchestration layer for rebuilding legally owned games. The current generation of the project centres on the C#/.NET 8 runtime **EngineNet**, which powers both a menu-driven CLI and an Avalonia desktop GUI. The legacy Python automation remains in the repository for historical context, but all active development targets EngineNet.

These docs explain how the engine is structured, how you can run it, and how to author new game modules that reuse its tooling.

## What the Engine Provides
- **Configuration-first workflows** written in TOML or JSON so you do not need to modify the core code for each game.
- **Embedded Lua and JavaScript interpreters** with shared helper modules for filesystem access, prompts, process execution, and SQLite integration.
- **Built-in helpers** that cover common extraction and conversion tasks (QuickBMS, TXD extraction, media transcoding, validation, directory flattening).
- **Extensible tool management** capable of downloading platform-specific binaries declared in module manifests.

## Target Audience
The project primarily serves module authors, preservationists, and developers who want predictable pipelines for asset extraction and rebuilds. End users interact with the simple GUI, whereas module authors typically use the CLI or the developer-centric command surface.

## Legal Context
Remake Engine never ships copyrighted assets. You must provide your own legally obtained game data. The engine is released for non-commercial educational and archival use; consult the [license](license_legal/license.md) for details.

## Contributing
Engine changes are driven by real modules. If you are building a module and find a missing capability, follow the contribution guide in [RemakeEngineDocs/src/CONTRIBUTING.md](CONTRIBUTING.md) and update the relevant specification in `EngineNet/specs/` alongside your code.

Enjoy exploring, extracting, and rebuilding.
