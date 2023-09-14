---
id: cf8jysrvqjkfxhklg0dfgr8
title: iterativeParse
desc: use a hand managed stack instead of recursivity?
updated: 1660214349531
created: 1660119483602
---


## backtracking


### Rule stack item

#### fini

backtrack ko

#### en cours

si Children[index] tous OK
    retourner SyntaxNode(children)
sinon
    index++

### clause Stack Item

______________________________________________

primary: INT
primary: LPAREN [d] expression RPAREN [d]

expression : term PLUS expression
expression : term MINUS expression
expression : term

term : factor TIMES term
term : factor DIVIDE term
term : factor

factor : primary
factor : MINUS factor


