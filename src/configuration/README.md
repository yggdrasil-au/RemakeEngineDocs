# Configuration Overview

Modules describe their behaviour through configuration files. The engine reads these at runtime, so no compilation step is required. This section documents the two main artefacts:

- `project.json` - user-specific settings stored at the repository root.
- `operations.json` / `operations.toml` - module manifests that list operations, prompts, and script metadata.

We also cover manifest structure, common fields, and an end-to-end example.
