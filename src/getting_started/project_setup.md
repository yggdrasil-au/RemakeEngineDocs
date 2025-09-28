# Project Setup (`project.json`)

`project.json` stores machine-specific settings such as project paths, default tool overrides, or user preferences. The engine creates a minimal file the first time you run it, but you can also craft one manually.

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
- Keep the file out of version control; it usually belongs in your personal `.gitignore`.
- Values can be referenced from manifests using placeholders such as `{{RemakeEngine.Directories.SourceRoot}}`.
- The engine reloads this file after each operation so scripts can persist changes when needed.
