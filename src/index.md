# Welcome to Remake Engine Documentation

[![Engine Version](https://img.shields.io/badge/version-2.22.1--alpha-blue.svg)](https://github.com/yggdrasil-au/RemakeEngine)
[![Commit Baseline](https://img.shields.io/badge/commit-bbbf037-green.svg)](https://github.com/yggdrasil-au/RemakeEngine/commit/bbbf037)
[![Target Framework](https://img.shields.io/badge/.NET-10.0%20LTS-purple.svg)](https://dotnet.microsoft.com/en-us/download/dotnet/10.0)

> **Baseline Version: Version 2.22.1 (Commit `bbbf037`)**  
> Dynamic Build Format: `T<Time>.D<Date>.C<Commit>.V<Version>` (generated dynamically at compile time via `Directory.Build.targets` from `.betterGit/project.toml`).

**Remake Engine** is an extensible, configuration-first orchestration engine and reverse-engineering pipeline framework for executing repeatable game reconstruction and asset extraction workflows. Built on a high-performance **.NET 10 LTS** core (**EngineNet**), the engine provides unified execution across an Avalonia-based cross-platform GUI, an interactive split-screen Terminal UI (TUI), and a developer-focused Command-Line Interface (CLI).

---

## Engine Capabilities Overview

- **17-Project Modular Architecture**: Cleanly partitioned across 5 architectural tiers (Foundational Data, Core Subsystems, Domain Services & Formats, Execution Runtimes, and User Interfaces) to ensure modular decoupling, testability, and fast execution.
- **Pure Managed Optical Disc Images Subsystem (`EngineNet.Core.DiscImages`)**: Pure C# sector parser supporting Xbox Original (XISO, XGD1), Xbox 360 (XGD2, XGD3), PlayStation 3 hybrid (ISO-9660/Joliet/UDF), PlayStation 2, and ECMA-119 ISO-9660 without external unmanaged dependencies.
- **Virtual Stream Slicing (`SubStream`)**: High-performance, seekable, non-allocating windowed byte slicing across multi-gigabyte disc images avoiding full disk extraction or memory blowup.
- **Categorized Game Module Registry**: Central registry (`EngineApps/Registries/Modules/Main.json`) supporting category containers (`Demo`, `SimpsonsGames`, `Lego`), category inheritance, and dynamic relocation matching between flat checkouts and categorized manifests.
- **Nested Submodule Integration**: Automatic recursive Git cloning (`--recurse-submodules`), BFS directory scanning up to depth 4, and directory pruning rules (`Source`, `GameFiles`, `reversing`, `TMP`, `.git`).
- **Polyglot Embedded Scripting**: First-class embedded Lua (MoonSharp 2.0) with rich SDK bindings (`iso`, `sdk.iso`, `sdk.extract_iso`, file hashing, path helpers), alongside JavaScript (Jint 4.17) and Python (IronPython 3.4.2).
- **Format Converters & Texture Codecs**: Built-in asset decoders for RenderWare TXD, Radical Pure3D (P3D), DirectDraw Surface (DDS), and BC1–BC7 texture compression.
- **External Tool Orchestration**: Automated downloader and execution manager for pipeline tools (QuickBMS, FFmpeg, vgmstream, ImageMagick) with placeholder path resolution.
- **Three Unified UX Modes**: Avalonia desktop GUI, split-screen TUI with category grouping headers (`=== [Category] ===`), and headless CLI for scripting and automated pipelines.

---

## Target Audience

This documentation is tailored for two primary audiences:

1. **Module Authors**: Developers writing game remake modules, defining operation manifests (`operations.toml`), setting up placeholders (`config.toml`), declaring required tools (`Tools.toml`), and scripting asset extractions using Lua or JavaScript.
2. **Engine Contributors**: Developers contributing to the engine codebase (`RemakeEngine.slnx`), implementing core services, format decoders, script engine bindings, or user interfaces.

---

## Documentation Structure

- **[Getting Started](getting-started/prerequisites.md)**: Setup prerequisites, build commands, test suite execution, and engine launch options.
- **[Architecture](architecture/core-runtime.md)**: Deep dive into the 17-project solution structure, optical disc image subsystem, execution pipeline, and script runtimes.
- **[Interfaces](interfaces/cli.md)**: Complete guides for the Command-Line Interface (CLI), interactive Terminal UI (TUI), and Avalonia Desktop GUI.
- **[Module Authoring](module-authoring/module-layout.md)**: Guide to authoring game modules, registry manifests, operation sequencing, prompts, placeholders, and tool manifests.
- **[Reference](reference/placeholders.md)**: Placeholder syntax reference and manifest JSON schemas.
- **[Contributing](CONTRIBUTING.md)**: Contribution guidelines and code style rules.

---

## Source of Truth Policy

When behavior in this book and source code disagree, treat the source code and official JSON schemas as canonical:

- `schemas/operations.schema.json` and `schemas/operations.toml.md` for operation structure
- `schemas/config.schema.json` for engine and module configuration
- `schemas/game.schema.json` for game module metadata and launch entry points
- `schemas/tools.schema.json` and `schemas/tools.toml.md` for external tool requirements
- `schemas/cli-command.schema.md` for CLI command syntax and patterns

---

## Legal and Asset Policy

Remake Engine does not distribute copyrighted game assets. All extraction and rebuilding workflows operate on legally obtained user disc images or archives. Modules targeting copyrighted games must comply with the Non-Commercial Module Policy documented in the repository root.
