# Operations Manifests (`operations.json` / `operations.toml`)

Each module exposes its workflows through an operations manifest. The engine supports:

- **JSON** ? historically used by early modules.
- **TOML** ? preferred for new work thanks to clearer grouping and multi-line support.

Both formats map group names to lists of operation objects. When no groups are defined the engine treats the manifest as a flat list.

```toml
# operations.toml

[[run-all]]
Name = "Extract archives"
script_type = "engine"
script = "format-extract"

[[custom]]
Name = "Custom Lua task"
script_type = "lua"
script = "Scripts/custom_task.lua"
args = ["--input", "{{Game_Root}}/source"]
```

```json
{
  "Example Game (PS3)": [
    {
      "Name": "Extract archives",
      "script_type": "engine",
      "script": "format-extract"
    }
  ]
}
```

The manifest, prompts, and optional placeholder TOML file (`config.toml`) define everything the engine needs to execute module workflows.
