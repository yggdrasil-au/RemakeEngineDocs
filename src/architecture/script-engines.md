# Script Engines

The ScriptEngines layer maps operation script types to executable actions.

## Runtime Families

- Embedded runtimes execute inside the engine process, such as lua, js, and python.
- External runtime wrappers launch tooling processes, such as bms workflows.

## Dispatch Model

ScriptActionDispatcher selects an IAction implementation based on script_type. Each action provides a consistent execution contract so runtime orchestration remains uniform.

## Lua API Definition Files

When new Lua globals are exposed by engine code, keep API definition files updated for editor tooling and documentation:

- EngineApps/Addons/RemakeEngineRuntime/library/api_definitions.lua
- EngineApps/api_definitions/api_definitions.d.ts
- EngineApps/api_definitions/api_definitions.pyi

Also update the demo Lua feature script when introducing new Lua-facing behavior.
