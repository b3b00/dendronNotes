---
id: Perso.projects.CSLY.generator
title: Perso.projects.CSLY.generator
desc: CSLY source generator
updated: 1765485032095
created: 0
---
# goal

Parser and lexer source generation.


__This should be a completely separate project__

# Plan

* [x] rebase branch project/aot-trimming/building
* [ ] working test project based on IndentedWhile parser
* [ ] "static parser" generation : see below
* [ ] "satic visitor" generation : see below

# Static parser

Generated parser **MUST NOT** rely on legacy CSLY runtime. So it should be a set of fully recursive  methods.
The Same goes for visitor.

## Opened questions

### models

We need a minimal set of shared model classes for :

* tokens
* syntax tree
* special EBNF types (Group, ValueOption)

**Tokens and ebnf types may be defined in csly generator assembly. Then the generator replaces the using vs. This easen the development. But maybe not that useful...**

How to include these types ?

* namespace :
  * static (e.g sly.model.generated)
  * by parser (e.g while.sly.model.generated)

Static namespace may produces conflicts at build if consuming assembly defines many parser. Using parser specific namespaces may produces too many classes.

If parser are in the same assembly there is no issue. Parser will share the same model. 
But we need a namespacing strategy to avoid conflicts between assemblies : 
 * use assembly name
 * use an argument on the`ParserGenerator` attribute

```csharp
[ParserGenerator("my.space")]
public class MyGenerator 
```

