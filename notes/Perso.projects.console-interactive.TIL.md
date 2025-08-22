---
id: Perso.projects.console-interactive.TIL
title: Perso.projects.console-interactive.TIL
desc: TIL
updated: 0
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
