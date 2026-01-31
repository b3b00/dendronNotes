---
id: Perso.projects.CSLY.csly-CLI.visitor
title: Perso.projects.CSLY.csly-CLI.visitor
desc: Minimal visitor
updated: 1769848409040
created: 0
---
# goal 
Allow csly grammar to generate a minimal grammar
Visitor can only generate strings.

# sample for JSON

```
[BroadenTokenWindow]
-> root : value => value_0;
value : STRING => value_0;
value : INT => value_0;
value : DOUBLE => value_0;
value : BOOLEAN => value_0;
value : NULL[d] => "NIL" ;
value : object => value_0;
value: list => value_0;
object: ACCG[d] ACCD[d] => "empty array";
object: ACCG[d] members ACCD[d] => Object( value_1);
list: CROG[d] CROD[d] => "empty object";
list: CROG[d] listElements CROD[d] => Array(value_1) ;
listElements: value additionalValue* => value_0 ", " value_1;
additionalValue: COMMA value => ", ";
members: property additionalProperty* => value_0 " " value_1";
additionalProperty : COMMA property => ", " value_1;
property: STRING COLON[d] value =>value_0 ", " value_2 ;
```
