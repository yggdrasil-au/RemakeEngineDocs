# Console Output Helpers

Earlier scripts used `Utils/printer.py`. EngineNet replaces this with `EngineSdk`, available to Lua and JavaScript through the injected `sdk` modules.

Key methods:
- `EngineSdk.Print(message, color?, newline?)` - Emits coloured console output and GUI notifications.
- `EngineSdk.Warn(...)` / `EngineSdk.Error(...)` - Structured warning and error events.
- `EngineSdk.Progress` - Creates a progress handle that updates the GUI and CLI in lockstep.
- `EngineSdk.Prompt(message, id, secret?)` - Requests user input when necessary.
- `EngineSdk.Emit(event, data?)` - Sends custom event payloads to downstream consumers.

Use these helpers instead of raw `Console.WriteLine` to ensure both interfaces stay in sync and follow the established colour scheme.
