# Installation

Clone the repository and restore dependencies using the .NET CLI. The examples below assume PowerShell, but the commands translate directly to other shells.

```pwsh
# Clone the repository
git clone https://github.com/yggdrasil-au/RemakeEngine.git
cd RemakeEngine

# Restore, build, and run tests
dotnet build RemakeEngine.sln
dotnet test RemakeEngine.sln --nologo
```

The solution compiles the EngineNet console project and its Avalonia GUI. After the initial build you can run either project directly from the command line or through your preferred IDE.
