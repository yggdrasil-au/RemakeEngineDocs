# Execution Flow

The operation pipeline is centered on manifest loading, context preparation, and dispatch.

## High-Level Sequence

1. Parse operations from operations.toml or operations.json.
2. Normalize operation metadata and IDs.
3. Collect user prompt answers from active interface.
4. Build runtime execution context from engine and module data.
5. Resolve placeholders recursively in script paths and arguments.
6. Execute selected operation or run-all flow.
7. Dispatch to built-in handlers or script runtimes.
8. Execute onsuccess child operations when parent succeeds.

## Run-All Behavior

Run-all mode includes operations marked with run-all or run_all and validates dependency order before dispatch.

## Placeholder Timing

For onsuccess child operations, placeholders are evaluated at child execution time, not frozen at parent start.

## Operational Impact

This flow lets module authors keep operations declarative while the runtime handles ordering, prompt input, and environment-specific value resolution.
