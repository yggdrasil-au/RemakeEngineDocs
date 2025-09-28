# Development Overview

Key principles guiding contributions:

- **Solve real module problems.** Engine work should originate from needs discovered while developing a concrete game module.
- **Keep specs current.** Every change to EngineNet must update the corresponding file in `EngineNet/specs/`. Specs explain purpose, invariants, TODOs, and known issues.
- **Prefer configuration changes over code edits.** Most module work involves editing manifests, scripts, and placeholder files.
- **Avoid regressions for other modules.** Run affected manifests and unit tests before submitting changes.

When adding new capabilities, capture them in both documentation and specs so other contributors understand how to reuse them.

Pull requests must pass the GitHub Actions pipelines described in [CI & Release Workflows](ci_workflows.md); keep `main` merged into your branch so cross-platform builds and SonarCloud analysis stay healthy.
