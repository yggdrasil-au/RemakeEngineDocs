# Contributing to Remake Engine Docs

The documentation lives in `RemakeEngineDocs/` and is published via mdBook. Contributions that align the docs with EngineNet or improve module author guidance are welcome.

## Workflow
1. Follow the main project guidelines in [`CONTRIBUTING.md`](../CONTRIBUTING.md).
2. Edit the Markdown sources inside `RemakeEngineDocs/src/`.
3. Update `SUMMARY.md` if you add, remove, or rename pages.
4. Build locally with `mdbook build RemakeEngineDocs` (optional) to preview changes.
5. Submit a pull request referencing the documentation updates alongside any code/spec changes.

## Style
- Use concise headings and plain ASCII text.
- Prefer TOML examples for new manifests; include JSON equivalents only when necessary.
- Link to specs or source files when describing engine behaviour.
- Keep the tone instructional and module-focused.

Thank you for keeping the docs accurate!
