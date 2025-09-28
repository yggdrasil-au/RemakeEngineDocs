# Adding a New Module

1. **Create the module directory** under `RemakeRegistry/Games/<ModuleName>/`.
2. **Author an operations manifest** (`operations.toml` preferred) to describe setup, extraction, conversion, and launch steps.
3. **Provide `config.toml` placeholders** if the operations depend on module-specific values.
4. **Add scripts or helper assets** under `Scripts/` or subfolders of your choosing.
5. **Document the module** with a `README.md` that lists prerequisites, operation notes, and expected outputs.
6. **Update `RemakeRegistry/Tools.json`** if you are introducing new downloadable tools.

Use the development checklists below to make sure the module is reproducible and friendly to other contributors.
