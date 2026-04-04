# Welcome to Remake Engine Documentation

Remake Engine is a configuration-first orchestration engine for repeatable game rebuild workflows. The active runtime is EngineNet, a .NET 10 application that powers CLI, TUI, and GUI experiences from one core execution pipeline.

This book is focused on two audiences:

- Module authors who define and maintain operations, placeholders, and tools.
- Engine contributors who need a reliable map of runtime flow and interface behavior.

## What You Can Do Here

- Learn how to build and run the engine locally.
- Author module manifests with TOML-first examples.
- Understand how operations are loaded, resolved, and dispatched.
- Learn how the shared serialization layer supports Core and script runtimes.
- Pick the right interface for your workflow: CLI, TUI, or GUI.
- Find schema references used for editor validation.

## Read in This Order

1. Start with Getting Started.
2. Continue with Module Authoring.
3. Use Architecture and Interfaces to understand behavior and execution paths.
4. Use Reference for placeholders and schema links.

## Source of Truth Policy

When behavior in this book and source code disagree, treat source files and schemas as canonical. In practice, that means keeping these areas in sync with runtime updates:

- schemas/operations.schema.json and schemas/operations.toml.md
- schemas/config.schema.json
- schemas/game.schema.json
- schemas/tools.schema.json and schemas/tools.toml.md

## Legal and Asset Policy

Remake Engine does not ship copyrighted game assets. You must provide your own legally obtained data.

## Contributing

Documentation updates are part of feature delivery. See [Documentation Contributions](CONTRIBUTING.md) for workflow and style rules.
