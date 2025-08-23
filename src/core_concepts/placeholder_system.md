# Placeholder System

Configuration files can reference dynamic values using `{{placeholder}}` syntax. Placeholders resolve from:

- `project.json` values
- the current game's metadata (`Game.Name`, `Game.RootPath`)
- answers from earlier prompts within the same operation

This allows modules to remain portable across environments without hardcoding paths.
