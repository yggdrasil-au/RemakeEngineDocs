# Prerequisites

Remake Engine targets .NET 8 and runs on Windows, macOS, and Linux. Before cloning the repository ensure the following tools are available:

- **.NET SDK 8.0 or newer** - required to build and run EngineNet and the Avalonia UI.
- **Git** - used to clone the repository and manage updates.
- **PowerShell 7+** (Windows) or a POSIX shell (macOS/Linux) - the examples in this guide use PowerShell syntax, but any shell capable of invoking `dotnet` will work.

For module authors, you will also need the external tools referenced by your operations (QuickBMS, FFmpeg, vgmstream, etc.). These can be acquired manually or via the Tools Downloader described later in this guide.
