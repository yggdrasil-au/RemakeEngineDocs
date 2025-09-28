# Using the Tools Downloader

Modules can declare their tool dependencies in a TOML manifest so the engine can download and verify them automatically. The downloader reads entries such as:

```toml
[[tool]]
name = "ffmpeg"
version = "6.1"
destination = "Tools/ffmpeg"
unpack = true
unpack_destination = "Tools/ffmpeg"
```

To process a manifest, call the built-in `download_tools` engine operation or run the `ToolsDownloader` API directly:

```pwsh
# Using the CLI run an operation that references download_tools
dotnet run --project EngineNet -- --game_module demo --script_type engine --script download_tools
```

Behind the scenes the downloader:
1. Reads the module TOML, collecting tool definitions.
2. Looks up platform-specific URLs and hashes in `RemakeRegistry/Tools.json` (fetching it from GitHub if missing).
3. Downloads archives with progress output and optional checksum verification.
4. Unpacks supported formats and updates `Tools.local.json` with resolved executable paths.
5. Obeys prompt answers such as `force_download` to control whether existing archives are re-fetched.

You can customise destinations, provide alternative mirrors, or force re-downloads via prompts exposed in the manifest.
