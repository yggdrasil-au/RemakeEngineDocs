# Game Manifest

game.toml declares how a built module launches its runtime target.

## Requirement

A game manifest must define exactly one launch target family:

- exe or executable
- lua, lua_script, or script
- godot, godot_project, or project

## Example: Executable

```toml
title = "My Remake"
exe = "{{Game_Root}}/bin/MyGame.exe"
```

## Example: Lua Entry

```toml
title = "My Remake"
lua = "{{Game_Root}}/scripts/entry.lua"
```

## Example: Godot Project

```toml
title = "My Remake"
godot = "{{Game_Root}}/project.godot"
```

## Notes

- Paths can include placeholders such as {{Game_Root}} and {{Project_Root}}.
- Keep title user-facing and descriptive.
- Validate manifest shape against schemas/game.schema.json.
