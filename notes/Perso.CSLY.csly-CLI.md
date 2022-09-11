cli for testing lexer and parser :

    1. from c# assembly
    2. from textual specification ( parsed with CSLY of course)


## from textual specification
ALA yacc/lex

### lexer
```
Generic:

[keyword]IF : if 

[String] STRING
[Int] INT
[Indentifier] : ID # to be derived for every identifier types


....
``` 

```
regex:

``` 

### Parser 

```
[Infix(LESSER,Right, 50)]
``` 
