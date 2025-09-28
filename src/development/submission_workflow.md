# Contribution Workflow

1. **Discuss first.** Open an issue describing the module or engine change you intend to make. Include context and desired outcomes.
2. **Create a feature branch.** Use `feature/<topic>` or `fix/<topic>` naming and keep the scope focused.
3. **Implement the change.** Update manifests, scripts, specs, and documentation together. For engine work ensure the relevant `.spec.md` files reflect the new behaviour.
4. **Run quality checks.**
   - `dotnet build RemakeEngine.sln`
   - `dotnet test RemakeEngine.sln`
   - Manual smoke tests for affected modules.
   - Review the [CI & Release Workflows](ci_workflows.md) guide to confirm you have mirrored the GitHub Actions checks locally.
5. **Open a pull request.** Summarise the change, list affected modules, and link the driving issue. Mention any follow-up work or TODOs left in specs.
6. **Respond to review.** Be prepared to iterate based on maintainer feedback. Keep discussions anchored to module needs.

Pull requests that alter engine behaviour without updating specs or documentation will be asked to do so before merging.
