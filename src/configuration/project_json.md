# Global Configuration (`project.json`)

`project.json` lets each contributor describe local paths, tool overrides, or other preferences without editing manifests. The engine loads it at startup and merges values into the placeholder context.

Recommended structure:

```json
{
  "RemakeEngine": {
    "Config": {
      "project_path": "C:/Projects/RemakeEngine"
    },
    "Directories": {
      "SourceRoot": "D:/Games/Example",
      "Extracted": "D:/RemakeOutput"
    },
    "Tools": {
      "ffmpeg": "C:/Tools/ffmpeg/bin/ffmpeg.exe"
    }
  }
}
```

Guidelines:
- Store absolute paths; manifests resolve them through placeholders like `{{RemakeEngine.Directories.SourceRoot}}`.
- Include optional settings your modules rely on, such as credentials or toggle flags.
- Keep the file out of git. Add it to your global excludes or repository `.gitignore`.
- Scripts can update values by writing to `project.json`; the engine reloads the file after each operation.
