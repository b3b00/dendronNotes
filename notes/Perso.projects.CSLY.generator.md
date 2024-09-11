---
id: Perso.projects.CSLY.generator
title: Perso.projects.CSLY.generator
desc: CSLY source generator
updated: 0
created: 0
---
# goal

Allow CSLy to work with AOT and trimming

# TIL

## source generator packaging

dot net is looking for generators (and analysers) in `nupkg#analysers/dotnet/cs`.
So the assembly MUST be copied to this folder

```xml
 <ItemGroup>
    <None Include="$(OutputPath)\netstandard2.1\$(AssemblyName).dll" Pack="true" PackagePath="analyzers/dotnet/cs" Visible="false" />
  </ItemGroup>
```
source : [incremental-generators-cookbook](https://github.com/dotnet/roslyn/blob/main/docs/features/incremental-generators.cookbook.md#incremental-generators-cookbook)
