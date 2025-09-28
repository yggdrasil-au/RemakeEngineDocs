# Writing Script Actions

## Lua
- Place scripts under `Scripts/` and reference them with `script_type = "lua"`.
- Use the injected globals (`tool`, `argv`, `emit`, `prompt`, `progress`) for engine integration.
- Require built-in modules such as `sdk.fs`, `sdk.json`, or `sdk.sql` for reusable helpers.

## JavaScript
- Use `script_type = "js"` to run scripts inside the Jint engine.
- The helper surface mirrors the Lua SDK, including tool resolution and EngineSdk wrappers.

## External executables
- For Python or third-party tools set `script_type` to the executable name (e.g., `python`, `quickbms`).
- Provide arguments via `args` and rely on placeholders to inject paths.

### Best practices
- Emit structured events (`EngineSdk.Emit`) instead of raw prints when you need the GUI to react.
- Fail fast with descriptive error messages; the engine turns non-zero exit codes into operation failures.
- Keep scripts idempotent when practical. Users often rerun operations while iterating on manifests.
