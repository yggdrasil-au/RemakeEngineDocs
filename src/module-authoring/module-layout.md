# Module Layout

A module lives under EngineApps/Games/ModuleName and is usually composed of:

- operations.toml for operation definitions
- config.toml for module-specific placeholders
- game.toml for launch metadata
- Tools.toml for tool dependencies
- scripts/ and operations/ for runtime scripts
- TMP/ for temporary generated data

## Minimal Module Skeleton

```text
EngineApps/Games/MyModule/
  operations.toml
  config.toml
  game.toml
  Tools.toml
  scripts/
  operations/
  TMP/
```

## Authoring Priorities

1. Define stable operation IDs and names first.
2. Keep prompts focused and explicit.
3. Move machine-specific values into config.toml placeholders.
4. Declare tools in Tools.toml instead of hardcoding binary paths.
