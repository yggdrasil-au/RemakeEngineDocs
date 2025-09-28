# Reading Output & Logs

EngineNet and its scripts emit two types of feedback:

1. **Plain console lines** ? free-form text written by scripts or built-in helpers. The CLI colours stderr red and stdout grey/cyan for readability.
2. **Structured events** ? JSON payloads prefixed with `@@REMAKE@@ ` produced by `EngineSdk`. The CLI parses and renders these events as:
   - `print` ? coloured messages with optional newline control.
   - `warning` / `error` ? emphasised notifications displayed in yellow or red.
   - `prompt` ? shown when a script requests additional input; the CLI forwards responses through the prompt system.
   - `progress` ? used by long-running tasks to update determinate progress bars (GUI) or concise textual updates (CLI).

When debugging:
- Enable verbose logging inside your manifests by passing additional args to scripts (e.g., `--debug`).
- Use structured events instead of ad-hoc prints when you want the GUI to react (progress bars, notifications, etc.).
- Review generated files or logs in `RemakeRegistry/Games/<Module>/` after each operation; the engine reloads configuration automatically so follow-up steps see the changes.
