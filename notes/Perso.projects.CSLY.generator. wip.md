---
id: Perso.projects.CSLY.generator. wip
title: Perso.projects.CSLY.generator. wip
desc: Todo
updated: 1764166467997
created: 0
---
## Lexer

* [x] keywords
* [x] long sugars
* [x] int and double
* [x] strings and chars
* [ ] indentation
* [ ] comments
* [ ] modes  : this will require to generate as many lexer as different mode. Plus a coordinator lexer

## BNF

* [x] parser
* [x] lexer
* [x] visitor
* [x] entrypoint
* [x] align names paced and naming
* [ ] errors
  * [ ] non terminal parse must iterate on ALL rules and store partial errors (see [RecursiveDescentSyntaxParser.NonTerminal.ParseNonTerminal()](https://github.com/b3b00/csly/blob/dev/src%2Fsly%2Fparser%2Fparser%2Fllparser%2Fbnf%2FRecursiveDescentSyntaxParser.NonTerminal.cs#L20) )

## EBNF

* [x] manies ( \* and + at first)
* [ ] groups
* [ ] choices (mandatory for expressions)
* [x] options
* [ ] expressions

