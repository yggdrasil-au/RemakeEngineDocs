# Managing External Tools

Most modules rely on third-party utilities such as QuickBMS, FFmpeg, or vgmstream. You can provide these tools manually or describe them in a module manifest so the engine can download them for you.

Recommended locations:
- Place community-shared tools in the repository-level `Tools/` directory.
- Keep module-specific scripts and binaries inside `RemakeRegistry/Games/<ModuleName>/`.

When resolving a tool path the engine:
1. Checks the `TOOLS_JSON` environment variable (if set) for a manifest.
2. Looks for `Tools.local.json` / `tools.local.json` in the project root.
3. Falls back to `RemakeRegistry/Tools.json`.
4. Returns the tool ID itself, allowing the OS `PATH` to resolve it.

Use `JsonToolResolver` manifests to point logical tool names (e.g., `ffmpeg`) to absolute paths on your machine. The [Tools Downloader](downloading_tools.md) covers automated acquisition based on module TOML files.
