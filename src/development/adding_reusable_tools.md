# Sharing Reusable Tools

If a tool benefits multiple modules:

1. Place archives or binaries under the top-level `Tools/` directory and document them in `RemakeRegistry/Tools.json`.
2. Provide a manifest entry describing download URL, checksum, and extraction details so the Tools Downloader can install it automatically.
3. Map the resolved executable path in `Tools.local.json` so scripts can call `tool("name")` without additional configuration.
4. Keep licences and attribution files with the tool where required.

For module-specific tools stick to the module directory to avoid polluting the shared workspace.
