# Script Engines & Lua Disc Image API

`EngineNet.ScriptEngines` provides embedded scripting environments and tool process dispatchers for operation steps. Rather than relying solely on external command-line shells, the engine hosts embedded runtime environments capable of deep interaction with core engine services, optical disc extraction, file validation, and format decoding.

---

## Script Action Dispatcher (`ScriptActionDispatcher`)

The `ScriptActionDispatcher` routes an operation step's `script_type` to its corresponding execution handler:

| `script_type` | Engine Runtime | Execution Model | Primary Purpose |
| :--- | :--- | :--- | :--- |
| **`lua`** | MoonSharp 2.0.0 | Embedded in-process interpreter | Primary scripting language for module authoring, disc extraction, asset transformations, and custom pipelines. |
| **`js`** / **`javascript`** | Jint 4.17.0 | Embedded in-process interpreter | Modern ECMAScript runtime for modules leveraging JavaScript logic and JSON manipulation. |
| **`python`** / **`py`** | IronPython 3.4.2 | Embedded in-process interpreter | In-process Python scripting for data structures, math, and rapid scripting. |
| **`engine`** | Native C# Handlers (`OpDispatcher`) | In-process compiled routines | High-performance built-in actions (`extract-iso`, `inspect-iso`, `format-extract`, `validate-files`, `rename-folders`). |
| **`bms`** | External Process Wrapper | QuickBMS invocation | Binary archive unpacking using BMS scripts. |
| **`internal`** | Internal Engine Tasks | System operations | Administrative engine workflows (e.g., `download_module_git`, `download_module_registry`). |

---

## Optical Disc Lua API (`iso` & `sdk.iso`)

The native disc image subsystem (`EngineNet.Core.DiscImages`) is bound to the MoonSharp Lua runtime in `LuaIso.cs`. It is accessible globally via the `iso` table and aliased through `sdk.iso` and `sdk.extract_iso`.

### Global Module Methods

#### `iso.open(path)`
Opens an optical disc image (`.iso`) for inspection, streaming, or extraction. Automatically normalizes host paths and checks permissions.

- **Parameters:**
  - `path` (`string`): Absolute or module-relative path to the `.iso` file.
- **Returns:**
  - `reader` (`IsoReader|nil`): Opened reader handle, or `nil` on failure.
  - `error` (`string|nil`): Diagnostic error string if opening failed.

```lua
local reader, err = iso.open("path/to/game.iso")
if not reader then
    error("Failed to open disc: " .. tostring(err))
end
```

#### `iso.is_iso(path)`
Fast, non-throwing probe that reads magic headers to detect if a file is a supported optical disc image.

- **Parameters:**
  - `path` (`string`): Path to candidate image file.
- **Returns:**
  - `is_disc` (`boolean`): `true` if a recognized optical disc signature was found.
  - `format` (`string|nil`): Detected format identifier (`"XGD3"`, `"XGD2"`, `"XGD1"`, `"XISO"`, `"PS3_HYBRID"`, `"PS2_ISO9660"`, `"STANDARD_ISO9660"`), or `nil`.

```lua
local is_valid, format = iso.is_iso("path/to/game.iso")
if is_valid then
    print("Found supported disc format: " .. format)
end
```

#### `iso.extract(iso_path, dest_dir [, options])` / `sdk.extract_iso(...)`
Convenience batch extraction helper that opens the image, extracts matching entries, and closes the handle in a single step.

- **Parameters:**
  - `iso_path` (`string`): Host path to `.iso` file.
  - `dest_dir` (`string`): Host output directory path.
  - `options` (`table|string`, optional): Glob pattern string (e.g. `"USRDIR/*"`) or `IsoExtractOptions` table.
- **Returns:**
  - `success` (`boolean`): `true` if extraction completed without fatal errors.
  - `error` (`string|nil`): Error description on failure.

```lua
local ok, err = iso.extract("game.iso", "./output", {
    filter = "*.xex",
    overwrite = true
})
```

---

## `IsoReader` Handle Methods (`LuaIsoHandle`)

Instances returned by `iso.open()` support both colon (`reader:method()`) and dot (`reader.method()`) calling conventions:

| Method | Return Type | Description |
| :--- | :--- | :--- |
| `reader:format()` | `string` | Returns detected disc filesystem format (e.g. `"XGD3"`, `"PS3_HYBRID"`). |
| `reader:volume_id()` | `string` | Returns volume descriptor label (e.g. `"PS3VOLUME"`, `"MICROSOFT*XBOX*MEDIA"`). |
| `reader:total_size()` | `integer` | Returns total size of the underlying ISO file in bytes. |
| `reader:partition_offset()` | `integer` | Active partition byte offset (e.g. `0x02080000` for XGD3, `0` for XISO). |
| `reader:exists(internal_path)` | `boolean` | Checks if a file or directory exists inside the image (case-insensitive). |
| `reader:get_entry(internal_path)` | `IsoEntry\|nil` | Retrieves metadata record for a file or directory. |
| `reader:list_files([subpath, recursive])` | `table<int, IsoEntry>` | Lists files and directories (default: `subpath = "/"`, `recursive = true`). |
| `reader:read_file(internal_path [, max_bytes])` | `string\|nil, string\|nil` | In-memory raw byte read via `SubStream` without disk extraction (Latin1 binary safe). |
| `reader:extract_file(internal_path, host_dest [, overwrite])` | `boolean, string\|nil` | Extracts a single file directly to a host destination. |
| `reader:extract(options)` | `IsoExtractResult` | Batch extracts matching files using an options table. |
| `reader:close()` / `reader:dispose()` | `void` | Closes underlying disc stream and releases file locks. |

### Data Structures (`IsoEntry`, `IsoExtractOptions`, `IsoExtractResult`)

#### `IsoEntry`
```lua
---@class IsoEntry
---@field path string       -- Normalized path inside disc (e.g. "PS3_GAME/PARAM.SFO")
---@field name string       -- Filename or directory name
---@field size integer      -- File size in bytes (0 for directories)
---@field is_dir boolean    -- True if entry is a directory
---@field sector integer    -- 2048-byte sector index relative to partition
---@field offset integer    -- Absolute byte offset in the underlying ISO file
```

#### `IsoExtractOptions`
```lua
---@class IsoExtractOptions
---@field destination string                     -- Output directory on host filesystem
---@field pattern? string                        -- Glob pattern (e.g. "USRDIR/*", "*.xex")
---@field filter? string                         -- Alias for pattern
---@field overwrite? boolean                     -- Overwrite existing files (default: false)
---@field progress_panel? PanelProgress          -- Engine progress sink
```

#### `IsoExtractResult`
```lua
---@class IsoExtractResult
---@field success boolean          -- True if completed without fatal error
---@field files_extracted integer  -- Total files written to disk
---@field files_skipped integer    -- Files skipped when overwrite is false
---@field bytes_extracted integer  -- Total bytes written
---@field error? string            -- Diagnostic error message if failed
```

---

## EmmyLua Type System & Annotation Requirements

To ensure complete VS Code IntelliSense, type checking, and maintainability across all Lua scripts and modules, the engine enforces strict EmmyLua annotations:

### 1. Mandatory Class Annotations (`---@class`)
Any table or object passed between functions, shared across files, or representing domain state must define a class schema:

```lua
---@class AssetMetadata
---@field id string
---@field source_path string
---@field target_path string
---@field file_size integer
---@field is_compressed boolean
```

### 2. Mandatory Function Annotations (`---@param`, `---@return`)
All functions must document parameter types and return values:

```lua
--- Processes an input texture and outputs a converted DDS file.
---@param input_path string Path to raw texture.
---@param output_dir string Target directory.
---@param format? "BC1"|"BC3"|"BC7" Texture compression format (default "BC7").
---@return boolean success True if conversion succeeded.
---@return string|nil error Error message on failure.
function ConvertTexture(input_path, output_dir, format)
    -- ...
end
```

### 3. Type Casting (`---@cast`)
When dealing with generic Lua tables, loops over file entries, or dynamic deserialization, use `---@cast` to restore type safety:

```lua
local files = reader:list_files("USRDIR", true)
for _, entry in ipairs(files) do
    ---@cast entry IsoEntry
    if not entry.is_dir and entry.name:match("%.sarc$") then
        print(string.format("Found archive: %s (%d bytes)", entry.path, entry.size))
    end
end
```

---

## API Definition Files Location

EmmyLua definition files are maintained under:

- `EngineApps/api_definitions/remake-engine/`: Core definition files:
  - `Custom_UserData.lua`: Object types (`IsoReader`, `IsoEntry`, `FileHandle`, `PanelProgress`).
  - `SDK Modules.lua`: Built-in SDK tables (`sdk.text`, `sdk.exec`, `sdk.iso`, `sdk.file`).
  - `Global Functions.lua`: Global helper routines (`import`, `prompt`, `log`).
  - `Global Constants.lua`: Predefined engine variables (`Game_Root`, `Project_Root`, `UIMode`).
- `EngineApps/api_definitions/api_definitions.lua`: Root IntelliSense entry point for VS Code language servers.
- `EngineApps/api_definitions/api_definitions.d.ts`: TypeScript declarations for JavaScript steps.
- `EngineApps/api_definitions/api_definitions.pyi`: Python stub files for Python steps.

When introducing new Lua global tables or SDK functions, contributors must update both the definition files under `EngineApps/api_definitions/` and the demonstration script in `EngineApps/Games/demo/scripts/lua_feature_demo.lua`.
