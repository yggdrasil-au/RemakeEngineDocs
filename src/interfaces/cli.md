# CLI

The CLI is designed for ad-hoc execution and automation. It can build an operation at runtime without requiring an existing operations.toml entry.

## Typical Use Cases

- CI automation
- One-off maintenance scripts
- Rapid testing of scripts before manifest integration

## Example Direct Invocation

```pwsh
dotnet run -c Debug --project EngineNet --framework net10.0 -- --game_module "./EngineApps/Games/demo" --script_type lua --script "{{Game_Root}}/scripts/lua_feature_demo.lua" --args '["--module", "{{Game_Root}}"]'
```

## Notes

- The CLI can bypass menu-driven operation selection.
- You can still list file-defined operations when needed.
- Favor direct CLI for repeatable automation entrypoints.
