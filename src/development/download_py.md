# Tool Distribution

`download.py` has been superseded by the `download_tools` engine action and the managed `ToolsDownloader` class.

Advantages of the new approach:
- Platform-aware downloads (Windows, Linux, macOS) pulled from `RemakeRegistry/Tools.json`.
- SHA256 verification with colour-coded feedback.
- Automatic extraction of supported archive formats and lock-file updates (`Tools.local.json`).
- Optional prompts that let users force re-downloads or skip expensive steps.

When updating modules, remove references to `download.py` and replace them with `script_type = "engine"` / `script = "download_tools"` operations.
