# Code Style & Conventions

- **C# code** follows the project defaults (`dotnet format` friendly, nullable enabled, explicit usings). Keep changes small and update matching specs.
- **Lua/JavaScript scripts** should favour readability over cleverness. Use descriptive function names, emit clear logs, and guard against missing files.
- **Configuration files** use spaces for indentation. TOML is preferred for new manifests; JSON remains supported for existing modules that still rely on it.
- **Commit messages** follow Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`, etc.).
- **Specs** in `EngineNet/specs/` must remain ASCII and include Purpose, Intent, Goals, Must Remain, Unimplemented, TODOs, and Issues.

Before submitting a pull request run `dotnet build` and `dotnet test` to ensure CI will pass.
