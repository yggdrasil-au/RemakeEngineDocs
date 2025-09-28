# Worked Example

The following TOML fragment demonstrates a complete operation that downloads tools, prompts for user input, and runs a Lua script.

```toml
[[setup]]
Name = "Download required tools"
script_type = "engine"
script = "download_tools"
prompts = [
  { Name = "force_download", type = "confirm", message = "Force re-download?", default = false }
]

[[process]]
Name = "Extract STR archives"
script_type = "lua"
script = "Scripts/extract_str.lua"
args = ["--input", "{{RemakeEngine.Directories.SourceRoot}}/archives", "--output", "{{Game_Root}}/Extracted"]
prompts = [
  { Name = "overwrite", type = "confirm", default = false, message = "Overwrite existing files?" }
]
```

- The first operation leverages the built-in downloader and uses a prompt to decide whether existing archives should be replaced.
- The second operation runs a Lua script with arguments resolved from placeholders and prompt answers (`{{overwrite}}`).
- Both operations will appear in the CLI under the `setup` and `process` headings respectively.
