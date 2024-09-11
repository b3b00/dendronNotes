---
id: Perso.projects.CSLY.generator
title: Perso.projects.CSLY.generator
desc: CSLY source generator
updated: 1726071406932
created: 0
---
# goal

Allow CSLY to work with AOT and trimming

## Fluent API



### lexer

for lexer 
```c#
public enum Lexer
{
    [AlphaId]
    ID,
    
    [Double]
    DOUBLE,
    
    [Keyword("YOLO")]
    YOLO
}
```

1. get a builder :

```c# 
var builder = AotLexerBuilder<Lexer>.NewBuilder()
```

2. add lexemes

```c#
.Double(Lexer.DOUBLE )
.Keyword(Lexer.YOLO , "YOLO")
```

## Generator

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
