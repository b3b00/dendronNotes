---
id: Perso.projects.CSLY.csly-CLI.logo
title: Perso.projects.CSLY.csly-CLI.logo
desc: logo parser
updated: 1744137533349
created: 0
---
```
program        = { instruction } ;

instruction    = command 
               | repeat 
               | procedure_definition 
               | procedure_call ;

command        = "AV" number             (* Avance *)
               | "RE" number             (* Recule *)
               | "TD" number             (* Tourne droite *)
               | "TG" number             (* Tourne gauche *)
               | "BC"                    (* Baisse crayon *)
               | "LC"                    (* Lève crayon *)
               | "NETTOIE"               (* Efface écran *)
               | "MAISON"                (* Centre la tortue *) ;

repeat         = "RÉPÈTE" number "[" { instruction } "]" ;

procedure_definition = "PO" identifier { parameter } 
                       { instruction } 
                       "FIN" ;

procedure_call = identifier { expression } ;

parameter      = ":" identifier ;
expression     = number | parameter ;

identifier     = letter { letter | digit } ;
number         = ["-"] digit { digit } ;
letter         = "A" | "B" | ... | "Z" | "a" | "b" | ... | "z" ;
digit          = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;

```


```
genericLexer logo;

[AlphaNumId] ID;
[Int] NUMBER;
[Keyword] AV : "AV";
[Keyword] RE : "RE";
[Keyword] TD : "TD";
[Keyword] TG : "TG";
[Keyword] BC : "BC";
[Keyword] LC : "LC";
[Keyword] CLEAN: "NETTOIE";
[Keyword] HOME : "MAISON";
[Keyword] REPEAT : "REPETE";
[Keyword] PO : "PO";
[Keyword] END : "FIN";
[Sugar] LBRACK : "[";
[Sugar] RBRANCK : "]";
[Sugar] COLON : ":";
parser logo;

program   :   instruction*

instruction  : [ command  | repeat   | procedure_definition | procedure_call ];

command  : [ AV | RE |TD | TG ]  NUMBER;
command : [ BC | LC];
command : [CLEAN | HOME]



repeat         : REPEAT NUMBER "[" instruction* "]" ;

procedure_definition : PO ID parameter* instruction* END;

procedure_call : identifier  expression*  ;

parameter    : COLON ID ;
expression  : NUMBER;
expression : | parameter ;


``` 

[share](https://b3b00.github.io/CslyViz/#Q1NMWS1NQVJLI2dlbmVyaWNMZXhlciBsb2dvOwoKW0FscGhhTnVtSWRdIElEOwpbSW50XSBOVU1CRVI7CltLZXlXb3JkXSBBViA6ICJBViI7CltLZXlXb3JkXSBSRSA6ICJSRSI7CltLZXlXb3JkXSBURCA6ICJURCI7CltLZXlXb3JkXSBURyA6ICJURyI7CltLZXlXb3JkXSBCQyA6ICJCQyI7CltLZXlXb3JkXSBMQyA6ICJMQyI7CltLZXlXb3JkXSBDTEVBTjogIk5FVFRPSUUiOwpbS2V5V29yZF0gSE9NRSA6ICJNQUlTT04iOwpbS2V5V29yZF0gUkVQRUFUIDogIlJFUEVURSI7CltLZXlXb3JkXSBQTyA6ICJQTyI7CltLZXlXb3JkXSBFTkQgOiAiRklOIjsKW1N1Z2FyXSBMQlJBQ0sgOiAiWyI7CltTdWdhcl0gUkJSQU5DSyA6ICJdIjsKW1N1Z2FyXSBDT0xPTiA6ICI6IjsKcGFyc2VyIGxvZ287CgotPiBwcm9ncmFtICAgOiAgIGluc3RydWN0aW9uKjsKCmluc3RydWN0aW9uICA6IFsgY29tbWFuZCAgfCByZXBlYXQgICB8IHByb2NlZHVyZV9kZWZpbml0aW9uIHwgcHJvY2VkdXJlX2NhbGwgXTsKCmNvbW1hbmQgIDogWyBBViB8IFJFIHxURCB8IFRHIF0gIE5VTUJFUjsKY29tbWFuZCA6IFsgQkMgfCBMQ107CmNvbW1hbmQgOiBbQ0xFQU4gfCBIT01FXTsKCgoKcmVwZWF0ICAgICAgICAgOiBSRVBFQVQgTlVNQkVSICJbIiBpbnN0cnVjdGlvbiogIl0iIDsKCnByb2NlZHVyZV9kZWZpbml0aW9uIDogUE8gSUQgcGFyYW1ldGVyKiBpbnN0cnVjdGlvbiogRU5EOwoKcHJvY2VkdXJlX2NhbGwgOiBJRCBleHByZXNzaW9uKiAgOwoKcGFyYW1ldGVyICAgIDogQ09MT04gSUQgOwpleHByZXNzaW9uICA6IE5VTUJFUjsKZXhwcmVzc2lvbiA6IHBhcmFtZXRlciA7JCMkaGVsbG8tez13b3JsZD19LWJpbGx5LXslIGlmIChhID09IDEpICV9LWJvYi17JWVsc2UlfS1ib3Vib3UteyVlbmRpZiV9dGhpcyBpcyB0aGUgZW5k)
