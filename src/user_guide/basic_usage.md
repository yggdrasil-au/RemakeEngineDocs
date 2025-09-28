# Typical Workflow

1. **Launch the engine** using either the GUI or the CLI.
2. **Select a module** from the detected entries in `RemakeRegistry/Games/`.
3. **Review prompts** configured for the chosen operation. Defaults can be provided in the manifest; answers are stored for the duration of the session and may populate placeholders.
4. **Run operations** sequentially. The GUI offers quick "run all" and launch buttons, while the CLI lets you trigger individual steps.
5. **Inspect results** in the console output or in the directories referenced by the manifest.
6. **Iterate** on manifests or scripts. The engine reloads `project.json` after each operation so any changes made by scripts are immediately visible to later steps.

Most modules include a `README.md` describing their operations. Consult it alongside the manifest to understand prerequisites and expected outputs.
