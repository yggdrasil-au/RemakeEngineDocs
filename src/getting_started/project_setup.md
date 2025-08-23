# Project Setup (`project.json`)

RemakeEngine stores user-specific paths and preferences in a `project.json` file placed at the repository root. If the file does not exist the engine will create one on first launch.

Example structure:
```json
{
  "RemakeEngine": {
    "Directories": {
      "SourcePath": "D:/GameData",
      "OutputPath": "D:/RemakeOutput"
    }
  }
}
```
Values from this file can be referenced in configuration using placeholders such as `{{RemakeEngine.Directories.SourcePath}}`.
