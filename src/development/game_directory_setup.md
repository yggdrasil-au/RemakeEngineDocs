# Directory Setup

A typical module layout looks like this:

```
RemakeRegistry/Games/MyGame/
?? operations.toml
?? config.toml
?? Scripts/
?  ?? extract.lua
?  ?? convert_audio.js
?? Tools/ (optional)
?  ?? custom_tool.exe
?? README.md
```

Guidelines:
- Use descriptive names that include the platform when necessary (`MyGame PS3`).
- Keep module-specific binaries in the module directory unless they are truly reusable.
- Add additional subfolders (`Assets/`, `Config/`, etc.) as needed; manifests can reference them via placeholders.
- Document any manual steps (e.g., copying disc files) in the module README.
