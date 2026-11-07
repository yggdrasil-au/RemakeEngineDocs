# Core Runtime & Modular Architecture

`EngineNet` is the runtime execution engine behind the Remake Engine framework. Built on **.NET 10 LTS**, the solution (`RemakeEngine.slnx`) comprises **17 specialized C# projects** structured across five architectural tiers, providing clean separation of concerns, rapid unit testing, and modular reuse across CLI, TUI, and GUI interfaces.

---

## 17-Project Architectural Tiers

The solution partitions responsibilities across five tiers plus the test suite:

```text
Tier 0: Core Abstractions & Data Models
  ├── EngineNet.Core.Abstractions  (Core contracts: IOperationLoader, IToolRunner, IGameLauncher)
  └── EngineNet.Core.Data          (Domain models: GameModules, Operation, Step, Prompt, Config)

Tier 1: Core Subsystems & Low-Level IO
  ├── EngineNet.Core               (Low-level IO routines and filesystem safety)
  └── EngineNet.Core.DiscImages    (Pure C# Optical Disc Image subsystem: ISO-9660, Joliet, UDF, XDVDFS)

Tier 2: Services, Media, Tools & Formats
  ├── EngineNet.Core.Services      (Manifest loader OperationsLoader, tool sync, module discovery)
  ├── EngineNet.Core.Operations    (Execution dispatcher OpDispatcher and built-in operation handlers)
  ├── EngineNet.Core.Engine        (GameLauncher and process lifecycle monitoring)
  ├── EngineNet.Core.ExternalTools (Tool acquisition ToolsDownloader, process runner, SharpCompress)
  ├── EngineNet.Core.Media         (Audio/video conversion pipeline, SQLite metadata indexing)
  ├── EngineNet.Core.Utils         (Placeholder resolver, folder renamer, file validator, Registries.cs)
  ├── EngineNet.GameFormats        (Assembly: EngineNet.Formats; TXD, P3D, DDS, BC1-BC7 block codecs)
  └── EngineNet.Shared             (Serialization: Tomlyn, YamlDotNet, JSON; LongPathIO; diagnostics)

Tier 3: Script Engines
  └── EngineNet.ScriptEngines      (ScriptActionDispatcher: Lua/MoonSharp, JS/Jint, Python/IronPython)

Tier 4: Interfaces & Host Executable
  ├── EngineNet.Interface          (Shared interface contracts, MiniEngineFace, async job queuing)
  ├── EngineNet.Interface.Terminal (Spectre.Console CLI and interactive Terminal.Gui TUI)
  ├── EngineNet.Interface.GUI      (Cross-platform Avalonia UI desktop application)
  └── EngineNet                    (Entry point executable Program.cs / Init.cs, mode routing)

Testing
  └── EngineNetTest                (EngineNetTest/EngineNetTest.csproj: 24 unit tests)
```

### Dependency Flow and Boundaries

The project dependency hierarchy flows downward strictly from host interfaces to core abstractions:

1. **Host Executable (`EngineNet`)**: Dispatches startup arguments to the appropriate interface (`GUI`, `TUI`, or `CLI`). References all interface, engine, and core projects.
2. **Interfaces (`EngineNet.Interface.*`)**: UI layers consume runtime functionality through `MiniEngineFace` and `Interface.Main` abstractions, decoupling UI controls from internal execution logic.
3. **Execution & Scripting (`Operations`, `Engine`, `ScriptEngines`)**: Sequenced by `OpDispatcher`. Script engines execute custom logic or invoke engine services via embedded SDK bindings.
4. **Services, Media & Formats**: Provide high-level business capabilities such as tool resolution, format decoding, and manifest parsing.
5. **Core Subsystems (`EngineNet.Core`, `EngineNet.Core.DiscImages`)**: Low-level sector-based disc reading, streaming, and IO safety.
6. **Data & Abstractions (`Core.Data`, `Core.Abstractions`, `Shared`)**: Zero-dependency domain models and serialization contracts.

---

## Optical Disc Images Subsystem (`EngineNet.Core.DiscImages`)

`EngineNet.Core.DiscImages` is a native, pure managed C# subsystem that auto-probes, reads, slices, and extracts console and PC optical disc images (`.iso`). It requires **zero unmanaged libraries, external executables, or drivers**.

### Supported Console Formats & Sector Geometry

Standard optical disc media use 2,048-byte physical sectors (`SectorSize = 2048`). Console images often place filesystem structures at specific security partition offsets:

| Format Identifier | Target Console / Media | Filesystem Type | Sector Size | Partition Base Offset | Volume Descriptor Offset | Magic / Signature |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`XGD3`** | Xbox 360 | XDVDFS (GDFX) | 2,048 bytes | `0x02080000` (Sector 16,640 / 32.5 MiB) | `0x02090000` (Base + Sector 32) | `"MICROSOFT*XBOX*MEDIA"` (20 bytes) |
| **`XGD2`** | Xbox 360 | XDVDFS (GDFX) | 2,048 bytes | `0x0FD90000` (Sector 129,824 / 253.5 MiB) | `0x0FDA0000` (Base + Sector 32) | `"MICROSOFT*XBOX*MEDIA"` (20 bytes) |
| **`XGD1`** | Xbox Original | XDVDFS (GDFX) | 2,048 bytes | `0x18300000` (Sector 198,144 / 387.0 MiB) | `0x18310000` (Base + Sector 32) | `"MICROSOFT*XBOX*MEDIA"` (20 bytes) |
| **`XISO`** | Xbox Original / 360 | XDVDFS (GDFX) | 2,048 bytes | `0x00000000` (Sector 0 / Trimmed Dump) | `0x00010000` (Sector 32) | `"MICROSOFT*XBOX*MEDIA"` (20 bytes) |
| **`PS3_HYBRID`** | PlayStation 3 | ISO-9660 + Joliet + UDF 2.50 Bridge | 2,048 bytes | `0x00000000` | Sector 16 (`0x8000`), Joliet Sector 17+, UDF Sector 20 (`0x9801` / `0xA001`) | PVD: `"CD001"`, System: `"PLAYSTATION"`, Volume: `"PS3VOLUME"` / UDF `"NSR02"`, `"NSR03"` |
| **`PS2_ISO9660`** | PlayStation 2 | ECMA-119 ISO-9660 | 2,048 bytes | `0x00000000` | Sector 16 (`0x8000`) | PVD: `"CD001"`, System: `"PLAYSTATION"`, presence of `SYSTEM.CNF` |
| **`STANDARD_ISO9660`** | PC / Generic | ECMA-119 ISO-9660 / Joliet | 2,048 bytes | `0x00000000` | Sector 16 (`0x8000`) | PVD: `"CD001"` |

### Subsystem Class Hierarchy

```text
IIsoReader (IDisposable)
  │
  └── IsoReaderBase (Abstract Base: Stream lifecycle, safety checks)
        ├── XdvdfsReader   (Xbox XISO, XGD1, XGD2, XGD3 partitions)
        └── Iso9660Reader  (PS3_HYBRID, PS2_ISO9660, STANDARD_ISO9660)

Supporting Primitives:
  ├── IsoFactory        (Format auto-probing, factory creation, CLI runner)
  ├── SubStream         (Virtual non-allocating Stream window)
  ├── IsoEntry          (File/directory metadata: path, name, size, sector, offset)
  ├── IsoExtractOptions (Destination, glob filter, overwrite, progress)
  ├── IsoExtractResult  (Extracted counts, byte tallies, error messages)
  └── IsoPathUtilities  (Glob matching, path normalization, scope verification)
```

### Virtual Slicing with `SubStream`

Extracting multi-gigabyte optical media to disk simply to read a few configuration files, textures, or executable headers is slow and disk-intensive. The disc subsystem introduces `SubStream`:
- **Windowed Streaming**: A seekable, read-only virtual `Stream` operating on an exact byte slice `[baseOffset, baseOffset + length]` of the underlying disc file.
- **Zero Allocation**: Avoids loading files into memory or copying byte arrays across boundaries.
- **Thread-Safe**: Synchronizes seeks and reads across concurrent consumers using an internal `_syncLock`.
- **Safe Lifecycle**: Disposing a `SubStream` releases the virtual window without closing the parent disc stream.
- **Lua In-Memory Reads**: Powers `reader:read_file(internal_path [, max_bytes])` to stream raw bytes directly into memory.

### Robust Filesystem Handling

- **ISO-9660 Level 3 Multi-Extent Stitching**: Standard 32-bit ISO-9660 limits individual directory extents to 4 GiB. For files exceeding 4 GiB (common on PS3 Blu-ray discs), `Iso9660Reader` detects the `FlagMultiExtent` bit (`0x80`) in directory records and transparently stitches sequential extents into a continuous stream.
- **Joliet UCS-2 Big Endian Decoding**: Automatically identifies Joliet Supplementary Volume Descriptors (SVD) containing UCS-2 Big Endian filenames, preserving mixed casing and long paths while falling back to standard PVD records on plain discs.
- **Xbox XDVDFS Table Parsing**: `XdvdfsReader` reads the 4-byte aligned directory table tree starting from the sector specified in the volume descriptor, correctly accounting for security partition lead-in offsets (`32.5 MiB` for XGD3, `253.5 MiB` for XGD2, `387.0 MiB` for XGD1).
- **Directory Traversal Protection**: `IsoReaderBase.ResolveSafeDestinationPath` sanitizes internal paths, rejects parent directory traversal (`..` or `.`), verifies destination boundary confinement via `Path.GetRelativePath`, and leverages `EngineNet.Shared.IO.LongPathIO` for Windows extended path lengths (`\\?\`).

---

## Operation Dispatch & Execution Pipeline

The core execution engine (`EngineNet.Core.Operations`) drives all automated tasks:

1. **Manifest Loading (`OperationsLoader`)**: Reads `operations.toml` or `operations.json`, populating `Operation` and `Step` domain models.
2. **Context Assembly (`ExecutionContext`)**: Combines module settings, machine-specific paths (`config.toml`), and runtime variables.
3. **Recursive Placeholder Resolution**: Recursively evaluates placeholder expressions (`{{Game_Root}}`, `{{Project_Root}}`, custom config keys).
4. **Step Sequencing (`OpDispatcher`)**: Routes steps to built-in handlers (`BuiltInOperations`) or script dispatchers (`ScriptActionDispatcher`).
5. **Interactive Prompts**: Prompts declared in manifests are presented to the active interface (GUI dialogs, TUI interactive prompts, or CLI automated responses via `--answer` or `--auto_prompt`).

---

## Unit Testing & Verification

The engine includes a dedicated MSTest suite in `EngineNetTest/EngineNetTest.csproj` containing **24 unit tests**:
- Validates disc image detection and sector geometry for Xbox and PlayStation formats.
- Verifies `SubStream` bounds checking and thread safety.
- Tests recursive module scanning, BFS traversal depth, and asset directory pruning.
- Tests RenderWare TXD and Radical P3D format decoding.

To execute the test suite:

```pwsh
dotnet test EngineNetTest/EngineNetTest.csproj --nologo
```
