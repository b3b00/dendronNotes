---
id: Perso.projects.CSLY.relaxed.visitor.typing
title: Perso.projects.CSLY.relaxed.visitor.typing
desc: new note
updated: 1758867454306
created: 0
---
# Goal
Allow relaxed visitor typing.
Actually visitor methods must be strongly typed with an unique return type.
Adding a more relaxed typing may help construct the visitor.


[issue #314](https://github.com/b3b00/csly/issues/314)

The parser type should still reflect the final result of a parse , i.e. the return type of the root rule is the output type. Parser.Parse then always returns the expected type and no cast is needed.



# Pain points

## Build

### EBNF : Groups / sub rules 

Groups generate hidden non terminals and rules. These use  

## Run

### Sub rules

### many rules (0+, 1+, {n,m})
