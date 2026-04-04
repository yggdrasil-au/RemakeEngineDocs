# Shared Library

EngineNet.Shared is the reusable support library for serialization helpers and other low-level utilities that need to be shared between Core and script-facing code.

## Responsibilities

- Provide TOML parsing and writing helpers for operations, config, and tool manifests.
- Provide JSON and YAML document helpers built on shared DOM conversion code.
- Provide diagnostics and logging helpers used by Core and script runtimes.
- Keep serialization logic independent from Core so Core can reference it without creating a cycle.

## Area Map

- `Serialization/Toml/`: TOML helpers built on Tomlyn.
- `Serialization/Json/`: JSON loading helpers and best-effort error handling.
- `Serialization/Yaml/`: YAML parsing and serialization helpers.
- `Serialization/DocModelConverter.cs`: shared JSON/TOML document conversion utilities.
- `Diagnostics.cs`: shared logging, trace, and exception logging helper.

## Relationship To Core

- Core consumes the shared serialization helpers for operations, config, registry, and script-adjacent loading.
- Core also consumes the diagnostics helper from this library so logging stays centralized without a project cycle.
- Shared remains a lower-level library so it can be referenced from Core without depending on Core itself.

## Related Docs

- [core-runtime.md](core-runtime.md)
- [../index.md](../index.md)