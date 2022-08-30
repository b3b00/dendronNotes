---
id: w9j2fscv265kn9tpf592npv
title: sourcegeneration
desc: ''
updated: 1661847365149
created: 1661844068516
---

# generic lexer performance

[github issue 306](https://github.com/b3b00/csly/issues/306)

Lexing is still the performance bottleneck in the global parsing process, if with the generic lexer :

![](/assets/images/2022-08-30-09-21-36.png)

Lexing may be improved by generating source code at build time [source code generator](https://docs.microsoft.com/fr-fr/dotnet/csharp/roslyn-sdk/source-generators-overview) or IL at runtime.

## 

That means generating a C#/IL representation of the generic lexer FSM.
This code should
   - manage backtracking
   - manage greatest match
   - manage position according to new line configuration.
        -  including [#177](https://github.com/b3b00/csly/issues/177)
   - preconditions
   - callbacks (extensibility) : 
       - may be an opportunity to simplify, implementing extension in a custom way ?
   - generate meaningful error message
   - use System.Memory to limit memory use

# case study

## lexer definition

```csharp
namespace jsonparser
{
    public enum JsonTokenGeneric
    {
        [Lexeme(GenericToken.String,channel:0)] STRING = 1,
        [Lexeme(GenericToken.Double,channel:0)] DOUBLE = 2,
        [Lexeme(GenericToken.Int,channel:0)] INT = 3,

        [Lexeme(GenericToken.KeyWord,channel:0, "true", "false")]
        BOOLEAN = 4,
        [Lexeme(GenericToken.SugarToken,channel:0, "{")] ACCG = 5,
        [Lexeme(GenericToken.SugarToken,channel:0, "}")] ACCD = 6,
        [Lexeme(GenericToken.SugarToken,channel:0, "[")] CROG = 7,
        [Lexeme(GenericToken.SugarToken,channel:0, "]")] CROD = 8,
        [Lexeme(GenericToken.SugarToken,channel:0, ",")] COMMA = 9,
        [Lexeme(GenericToken.SugarToken,channel:0, ":")] COLON = 10,
        [Lexeme(GenericToken.KeyWord,channel:0, "null")] NULL = 14
    }
}
```

## fsm

![](/assets/images/graphviz.svg)

## source generation

### nodes

Generate a C# enum whose values are the FSM nodes.

```csharp
public enum Nodes {
    Start = 0,
    
    InIdentifier = 1,
    InInt = 2,

    StartDouble = 3,
    InDouble = 4,

    InString1 = 5,
    EscapeString1 = 6,
    EndString1 = 7,

    Sugar8 = 8,

    Sugar9 = 9,

    Sugar10 = 10,

    Sugar11 = 11,

    Sugar12 = 12,

    Sugar13 = 13

}
```


### transitions






