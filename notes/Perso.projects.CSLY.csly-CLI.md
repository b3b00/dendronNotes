---
id: ts1k8crs82j8fxdag3j4389
title: Perso.projects.CSLY.csly-CLI
desc: CLI Companion tool for CSLY
updated: 1713510398628
created: 1659685379252
---

[CLSY CLI tool](https://github.com/b3b00/cslycli)

cli for testing lexer and parser :

    1. from c# assembly
    2. from textual specification ( parsed with CSLY of course)


## from textual specification

The tester generates a dynamic enum and parser class. than it uses it to build the parser with the csly runtime.
  - [generate enum](https://stackoverflow.com/questions/857414/dynamically-create-an-enum)
  - [generate method](https://stackoverflow.com/questions/1080034/creating-a-function-dynamically-at-run-time)

ALA yacc/lex

### lexer
```
Generic:

[keyword]IF : if 

[String] STRING
[Int] INT
[AlphaId] : ID # to be derived for every identifier types


[ Sugar] IF:if
[ Sugar] THEN:then
[ Sugar] ELSE:else
[ Sugar] WHILE:while
[ Sugar] DO:do
[ Sugar] SKIP:skip
[ Sugar] TRUE:true
[ Sugar] FALSE:false
[ Sugar] NOT:not
[ Sugar] AND:and
[ Sugar] OR:or
[ Sugar] PRINT:print

 [Sugar] GREATER : >

[Sugar] LESSER : <

[Sugar] EQUALS : ==

[Sugar] DIFFERENT : !=

[Sugar] CONCAT : .

[Sugar]] ASSIGN : :=

[Sugar]] PLUS : +
[Sugar] MINUS : -
[Sugar] TIMES : *
[Sugar] DIVIDE : /

[Sugar] LPAREN : ()
[Sugar] RPAREN : )
[Sugar] SEMICOLON : ;
....
``` 

```
regex:

``` 

### Parser

```
# operations 

[Right 50] LESSER
[Right 50] GREATER
[Right 50] EQUALS
[Right 50]DIFFERENT

[Right 10] CONCAT
       
[Right 10] PLUS
[Left 10] MINUS
[Right 50] TIMES
[Left 50]DIVIDE

[Prefix 100] MINUS

[Right 10] OR
[Right 50] AND
[Prefix 100] NOT

# operands

[Operand] INT
[Operand] TRUE
[Operand] FALSE
[Operand] STRING
[Operand] ID

# statements

statement :  LPAREN statement RPAREN 

statement : sequence

sequence : statementPrim additionalStatements*

additionalStatements : SEMICOLON statementPrim

statementPrim: IF WhileParserGeneric_expressions THEN statement ELSE statement

statementPrim: WHILE WhileParserGeneric_expressions DO statement

statementPrim: IDENTIFIER ASSIGN WhileParserGeneric_expressions

statementPrim: SKIP

statementPrim: PRINT parser_expressions

``` 
