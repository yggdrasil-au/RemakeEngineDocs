# CI & Release Workflows

Remake Engine relies on GitHub Actions to guard quality on every pull request and to publish tagged releases. Understanding what each workflow does helps you debug failures quickly and mirror the same steps locally.

## Pull Request & `main` Branch Pipelines
- **`SonarQube.yml`** — Runs on Windows. Restores dependencies, builds the solution, executes `dotnet test` with OpenCover output, and publishes the results to SonarCloud (`EngineNet.Tests/TestResults`). Treat this as the canonical CI check.
- **`build.yml`** — Also runs on Windows. Performs a build under the runner-provided Sonar scanner cache as a faster redundancy when SonarCloud has intermittent hiccups.
- **`SonarQubeBuild.yml`** — Runs the SonarSource scan action on Ubuntu so analysis continues to work from a Linux host. It reuses the same project key and exclusions as the Windows pipelines.

If any of these fail, reproduce the problem locally with:

```pwsh
dotnet build RemakeEngine.sln
dotnet test RemakeEngine.sln --nologo --collect "XPlat Code Coverage"
```

Coverage artifacts from the CI live under `EngineNet.Tests/TestResults/` and are consumed automatically by SonarCloud. Delete the folder before re-running locally if you want a clean slate.

## Release Pipelines
- **`on tagged release -- Win,Linux,Mac .NET Test, Build, Release.yml`** — Kicks off when you push a tag that matches `v*` (for example `v2.5.0`). The workflow builds and tests Debug/Release binaries across Windows, macOS, and Linux, then publishes self-contained archives for `win-x64`, `win-arm64`, `linux-x64`, `linux-arm64`, `osx-x64`, and `osx-arm64`. The job finishes by creating a GitHub Release and uploading each archive.
- **`on win tagged release -- Win64 .NET Build & Test.yml`** — Triggers on tags matching `win-v*` or when executed manually. It concentrates on Windows-only Debug/Release builds/tests and publishes a single `win-x64` archive to a GitHub Release.

Coordinate with maintainers before cutting a tag so the required secrets (`SONAR_TOKEN`, `SUPERSECRET_TOKEN`, etc.) are available. Tags are immutable; use `git push --delete origin <tag>` if you need to retry.

## Tips for Contributors
- Keep your feature branches up to date with `main` to avoid failing because of stale Sonar analysis context.
- Link to the relevant workflow run when asking for review help; it saves everyone a trip through the Actions UI.
- When you need to re-run CI, prefer the "Re-run failed jobs" option to reuse caches and reduce queue time.
