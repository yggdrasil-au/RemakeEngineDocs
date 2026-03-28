# Schemas

Use schemas for editor validation, completion, and consistent module authoring.

## Core Schema Files

- schemas/operations.schema.json: operation format validation
- schemas/config.schema.json: module placeholder structure
- schemas/game.schema.json: launch target manifest structure
- schemas/tools.schema.json: tool dependency manifest validation

## Companion Markdown Specifications

- schemas/operations.toml.md: operations authoring guide and prompt behavior
- schemas/tools.toml.md: tools manifest behavior and runtime policy

## Sync Policy

When runtime parsing behavior changes in engine code, update matching schema and documentation files in the same change set.

This policy keeps IDE validation accurate and avoids drift between module author documentation and runtime behavior.
