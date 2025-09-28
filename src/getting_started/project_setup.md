# Project Setup (`project.json`)

`project.json` stores machine-specific settings such as project paths, default tool overrides, or user preferences. EngineNet creates a minimal file the first time it starts (or when a script calls `sdk.ensure_project_config`), but you can replace it with a richer configuration at any time.

Example skeleton:

```json
{
  "RemakeEngine": {
    "Config": {
      "project_path": "C:/Projects/RemakeEngine"
    },
    "Directories": {
      "SourceRoot": "D:/Games/Example"
    },
    "Tools": {
      "ffmpeg": "C:/Tools/ffmpeg/bin/ffmpeg.exe"
    }
  }
}
```

Guidelines:
- Keep the file out of version control; add it to your personal `.gitignore`.
- Values are available to manifests through placeholders such as `{{RemakeEngine.Directories.SourceRoot}}`.
- Scripts can update settings by writing to `project.json`; the engine reloads it after every operation so later steps see the changes.
- `CommandBuilder` also projects `module_path` and `project_path` into `RemakeEngine.Config` automatically, so scripts that expect those legacy keys continue to work.
