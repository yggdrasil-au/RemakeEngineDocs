# Managing External Tools

Most modules rely on third-party utilities such as QuickBMS, FFmpeg, or vgmstream. You can stage these tools manually or describe them in a module manifest so the engine can download them for you.

Recommended locations:
- Place community-shared tools in the repository-level `Tools/` directory.
- Keep module-specific scripts and binaries inside `RemakeRegistry/Games/<ModuleName>/`.

When resolving a tool path EngineNet checks sources in this order:
1. A JSON manifest pointed to by the `TOOLS_JSON` environment variable.
2. `Tools.local.json` or `tools.local.json` in the project root.
3. `RemakeRegistry/Tools.json` (or `tools.json`) downloaded automatically from the upstream repo if missing.
4. Falling back to the tool name itself, allowing the system `PATH` or caller to resolve it (`PassthroughToolResolver`).

Use `JsonToolResolver` manifests to map logical tool names (for example `ffmpeg`) to absolute paths on your machine. The [Tools Downloader](downloading_tools.md) can populate these manifests and keep checksums in sync with module TOML files.
