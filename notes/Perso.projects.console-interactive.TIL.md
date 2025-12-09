---
id: Perso.projects.console-interactive.TIL
title: Perso.projects.console-interactive.TIL
desc: TIL
updated: 1765309915242
created: 0
---
# nuget source generator et runtime
Si un nuget embarque à la fois un source generator et des classes runtime alors il faut absolument que `<IncludeBuildOutput>true</IncludeBuildOutput>`

```xml
<PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
      <PublishRepositoryUrl>true</PublishRepositoryUrl>
      <EmbedUntrackedSources>true</EmbedUntrackedSources>           <AllowedOutputExtensionsInPackageBuildOutputFolder>$(AllowedOutputExtensionsInPackageBuildOutputFolder);.pdb</AllowedOutputExtensionsInPackageBuildOutputFolder>
    <Authors>b3b00</Authors>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <PackageProjectUrl>https://github.com/b3b00/interactiveCLI</PackageProjectUrl>
    <RepositoryUrl>https://github.com/b3b00/interactiveCLI</RepositoryUrl>   
    <version>1.0.6</version>
    <PackageVersion>1.0.6</PackageVersion>
    <LangVersion>latest</LangVersion>   
    <PublishRepositoryUrl>true</PublishRepositoryUrl>
    <EmbedUntrackedSources>true</EmbedUntrackedSources>
    <PackageOutputPath>./nupkg</PackageOutputPath>
<!-- inclue les symboles -->
    <IncludeSymbols>true</IncludeSymbols>
    <SymbolPackageFormat>snupkg</SymbolPackageFormat>
    <ContinuousIntegrationBuild>true</ContinuousIntegrationBuild>
<!-- analyser roslyn ou source generator -->
      <IsRoslynComponent>true</IsRoslynComponent>
<!-- publie les binaires dans le package -->
      <IncludeBuildOutput>true</IncludeBuildOutput>
  </PropertyGroup>

```


# debugging console app with VS Code

Debugging a console app that access console cursor position `Console.GetCursorPosition()` crashes with an `invalid handle` error.

This is caused by the VS Code internal console. 

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [

        {
            "name": "test",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            "program": "${workspaceFolder}/src/testproject/bin/Debug/net9.0/testproject.dll",
            "args": [""],
            "cwd": "${workspaceFolder}",
            "console": "internalConsole",
            "stopAtEntry": false
        }
    ]
}
```

To solve : use an external terminal instead

```` json
{   
    "version": "0.2.0",
    "configurations": [

        {
            "name": "test",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            "program": "${workspaceFolder}/src/testproject/bin/Debug/net9.0/testproject.dll",
            "args": [""],
            "cwd": "${workspaceFolder}",
// use external terminal instead of internal console
            "console": "externalTerminal",
            "stopAtEntry": false
        }
    ]
}



