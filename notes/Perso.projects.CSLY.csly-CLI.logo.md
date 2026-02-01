---
id: Perso.projects.CSLY.csly-CLI.logo
title: Perso.projects.CSLY.csly-CLI.logo
desc: logo parser
updated: 1769933483919
created: 0
---
tutle all the way down.

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
genericLexer logolexer;

[AlphaNumId] ID;
[Int] NUMBER;
[KeyWord] AV : "AV";
[KeyWord] RE : "RE";
[KeyWord] TD : "TD";
[KeyWord] TG : "TG";
[KeyWord] BC : "BC";
[KeyWord] LC : "LC";
[KeyWord] CLEAN: "NETTOIE";
[KeyWord] HOME : "MAISON";
[KeyWord] REPEAT : "REPETE";
[KeyWord] PO : "PO";
[KeyWord] END : "FIN";
[Sugar] LBRACK : "[";
[Sugar] RBRANCK : "]";
[Sugar] COLON : ":";
parser logoparser;

-> program   :   instruction*;

instruction  : [ command  | repeat   | procedure_definition | procedure_call ];

command  : [ AV | RE |TD | TG ]  expression;
command : [ BC | LC];
command : [CLEAN | HOME];



repeat         : REPEAT NUMBER "[" instruction* "]" ;

procedure_definition : PO ID parameter* instruction* END;

procedure_call : ID expression*  ;

parameter    : COLON ID ;
#expression : [expression_number | expression_parameter];
expression  : NUMBER;
expression : parameter ;

``` 

[Logo CSLY Viewer](https://b3b00.github.io/CslyViz/grammar#Q1NMWS1NQVJLI2dlbmVyaWNMZXhlciBsb2dvbGV4ZXI7CgpbQWxwaGFOdW1JZF0gSUQ7CltJbnRdIE5VTUJFUjsKW0tleVdvcmRdIEFWIDogIkFWIjsKW0tleVdvcmRdIFJFIDogIlJFIjsKW0tleVdvcmRdIFREIDogIlREIjsKW0tleVdvcmRdIFRHIDogIlRHIjsKW0tleVdvcmRdIEJDIDogIkJDIjsKW0tleVdvcmRdIExDIDogIkxDIjsKW0tleVdvcmRdIENMRUFOOiAiTkVUVE9JRSI7CltLZXlXb3JkXSBIT01FIDogIk1BSVNPTiI7CltLZXlXb3JkXSBSRVBFQVQgOiAiUkVQRVRFIjsKW0tleVdvcmRdIFBPIDogIlBPIjsKW0tleVdvcmRdIEVORCA6ICJGSU4iOwpbU3VnYXJdIExCUkFDSyA6ICJbIjsKW1N1Z2FyXSBSQlJBTkNLIDogIl0iOwpbU3VnYXJdIENPTE9OIDogIjoiOwpwYXJzZXIgbG9nb3BhcnNlcjsKCi0+IHByb2dyYW0gICA6ICAgaW5zdHJ1Y3Rpb24qOwoKaW5zdHJ1Y3Rpb24gIDogWyBjb21tYW5kICB8IHJlcGVhdCAgIHwgcHJvY2VkdXJlX2RlZmluaXRpb24gfCBwcm9jZWR1cmVfY2FsbCBdOwoKY29tbWFuZCAgOiBbIEFWIHwgUkUgfFREIHwgVEcgXSAgZXhwcmVzc2lvbjsKY29tbWFuZCA6IFsgQkMgfCBMQ107CmNvbW1hbmQgOiBbQ0xFQU4gfCBIT01FXTsKCgoKcmVwZWF0ICAgICAgICAgOiBSRVBFQVQgTlVNQkVSICJbIiBpbnN0cnVjdGlvbiogIl0iIDsKCnByb2NlZHVyZV9kZWZpbml0aW9uIDogUE8gSUQgcGFyYW1ldGVyKiBpbnN0cnVjdGlvbiogRU5EOwoKcHJvY2VkdXJlX2NhbGwgOiBJRCBleHByZXNzaW9uKiAgOwoKcGFyYW1ldGVyICAgIDogQ09MT04gSUQgOwojZXhwcmVzc2lvbiA6IFtleHByZXNzaW9uX251bWJlciB8IGV4cHJlc3Npb25fcGFyYW1ldGVyXTsKZXhwcmVzc2lvbiAgOiBOVU1CRVI7CmV4cHJlc3Npb24gOiBwYXJhbWV0ZXIgOyQjJFBPIENBUlJFIDpMT05HVUVVUgogIFJFUEVURSA0IApbIApBViA6TE9OR1VFVVIgClREIDkwIApdCkZJTgpDQVJSRSAxMDAK)
