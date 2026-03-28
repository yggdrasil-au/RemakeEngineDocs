# Core Runtime

EngineNet.Core is the runtime layer behind all interfaces. It provides loading, context assembly, placeholder resolution, and operation execution.

## Core Responsibilities

- Track runtime state shared by CLI, TUI, and GUI.
- Discover modules from registries and game directories.
- Load operation manifests from operations.toml or operations.json.
- Collect prompt answers and combine with defaults.
- Build execution context from engine config, module metadata, and config.toml placeholders.
- Resolve placeholders recursively in operation payloads.
- Dispatch operations to built-in handlers and script engines.

## Runtime Area Map

- Engine/: operation execution entry points
- Services/: operation loading, command execution, launcher, and registry services
- Data/: runtime models
- Utils/: execution context and placeholder resolvers
- Serialization/: TOML, JSON, and YAML parsing helpers
- ExternalTools/: tool acquisition and resolution support

## Interface Boundary

Interfaces call into core through a narrowed contract exposed by Interface.Main. This protects UI layers from direct deep coupling to runtime internals.
