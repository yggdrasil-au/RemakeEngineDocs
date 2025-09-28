# Introduction

Remake Engine helps module authors rebuild classic games by orchestrating extraction and conversion pipelines. The project centres on the C#/.NET 8 runtime **EngineNet**, which powers both a menu-driven CLI and an Avalonia desktop GUI. Everything the engine does is declared in configuration files so game-specific logic stays outside of the core binaries.

## Philosophy
- **Module-first development:** Engine work is triggered by real module needs. Features are not added speculatively.
- **Configuration over code:** Operations are described in TOML or JSON manifests, keeping custom logic in scripts or built-in helpers.
- **Safety and reproducibility:** The engine avoids destructive actions unless a manifest explicitly asks for them and always resolves paths through placeholders.
- **Community reuse:** Common tools, prompts, download manifests, and helper modules live in shared locations so the broader community can benefit from them.

## What to Read Next
- [Getting Started](../getting_started/README.md) for environment setup.
- [Core Concepts](../core_concepts/README.md) to understand how configuration, placeholders, and built-in operations fit together.
- [Development](../development/README.md) for module authors expanding the ecosystem.
