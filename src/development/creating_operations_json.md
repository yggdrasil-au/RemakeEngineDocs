# Authoring Operation Manifests

When building the manifest:

1. **Start with groups.** Organise related operations under headings (`[[prepare]]`, `[[extract]]`, etc.) to keep large manifests readable.
2. **Mix built-in and scripted tasks.** Use `script_type = "engine"` for common helpers and `lua`/`js` for bespoke logic.
3. **Factor reusable prompts** into helper tables so you can reuse them across operations.
4. **Leverage placeholder resolution** rather than hardcoding paths. Combine global (`project.json`) and module (`config.toml`) values.
5. **Consider run-all behaviour.** Mark frequently executed operations with `run-all = true` so they participate in the one-click workflow.

Always validate the manifest by running it end-to-end. The CLI?s stream output and prompt summaries help catch typos early.
