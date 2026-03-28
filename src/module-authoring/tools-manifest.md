# Tools Manifest

Tools.toml declares module tool dependencies. The engine installs tools into shared project-level locations and reuses them across modules.

## Minimal Example

```toml
title = "Module Tools"

[[tool]]
name = "QuickBMS"
version = "0.12.0"
unpack = true

[[tool]]
name = "ffmpeg"
version = "8.0"
unpack = true
```

## Runtime Policy

- Tool archives are cached under EngineApps/Tools/_archives.
- Installed tools are placed under EngineApps/Tools.
- Install folders are namespaced by tool, version, and platform.

Example folder naming:

- QuickBMS-0.12.0-win-x64
- Blender-4.5.4-win-arm64

## Compatibility Notes

Legacy fields destination and unpack_destination are accepted for compatibility but ignored by current runtime placement logic.

## Canonical Reference

Use schemas/tools.toml.md and schemas/tools.schema.json as the source of truth for supported fields.
