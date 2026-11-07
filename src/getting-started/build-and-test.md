# Build and Test

Use the solution-level commands from repository root to validate engine changes.

## Build

```pwsh
dotnet build RemakeEngine.slnx
```

## Test

```pwsh
dotnet test EngineNetTest/EngineNetTest.csproj --nologo
```

## Optional Coverage Collection

```pwsh
dotnet test EngineNetTest/EngineNetTest.csproj --collect "XPlat Code Coverage"
```

## Build Documentation Locally

From repository root:

```pwsh
mdbook build RemakeEngineDocs
```

If mdbook is not installed globally, you can use the bundled executable in RemakeEngineDocs.
