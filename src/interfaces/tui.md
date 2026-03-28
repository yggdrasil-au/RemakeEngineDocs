# TUI

The TUI is a menu-driven terminal interface that executes workflows defined in operation manifests.

## Behavior

1. Discover available modules.
2. Load operations from module manifests.
3. Present selectable operations.
4. Collect prompt input interactively.
5. Stream output as operations run.

## Best For

- Interactive development sessions
- Quick validation of module operation design
- Running workflows without GUI dependencies

## Constraint

The TUI is file-driven. A script must exist in the operation manifest to be executed through this interface.
