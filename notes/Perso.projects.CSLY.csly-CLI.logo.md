---
id: Perso.projects.CSLY.csly-CLI.logo
title: Perso.projects.CSLY.csly-CLI.logo
desc: logo parser
updated: 1744138389137
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

[Logo CSLY Viewer](https://b3b00.github.io/CslyViz/grammar#Q1NMWS1NQVJLI2dlbmVyaWNMZXhlciBsb2dvbGV4ZXI7CgpbQWxwaGFOdW1JZF0gSUQ7CltJbnRdIE5VTUJFUjsKW0tleVdvcmRdIEFWIDogIkFWIjsKW0tleVdvcmRdIFJFIDogIlJFIjsKW0tleVdvcmRdIFREIDogIlREIjsKW0tleVdvcmRdIFRHIDogIlRHIjsKW0tleVdvcmRdIEJDIDogIkJDIjsKW0tleVdvcmRdIExDIDogIkxDIjsKW0tleVdvcmRdIENMRUFOOiAiTkVUVE9JRSI7CltLZXlXb3JkXSBIT01FIDogIk1BSVNPTiI7CltLZXlXb3JkXSBSRVBFQVQgOiAiUkVQRVRFIjsKW0tleVdvcmRdIFBPIDogIlBPIjsKW0tleVdvcmRdIEVORCA6ICJGSU4iOwpbU3VnYXJdIExCUkFDSyA6ICJbIjsKW1N1Z2FyXSBSQlJBTkNLIDogIl0iOwpbU3VnYXJdIENPTE9OIDogIjoiOwpwYXJzZXIgbG9nb3BhcnNlcjsKCi0+IHByb2dyYW0gICA6ICAgaW5zdHJ1Y3Rpb24qOwoKaW5zdHJ1Y3Rpb24gIDogWyBjb21tYW5kICB8IHJlcGVhdCAgIHwgcHJvY2VkdXJlX2RlZmluaXRpb24gfCBwcm9jZWR1cmVfY2FsbCBdOwoKY29tbWFuZCAgOiBbIEFWIHwgUkUgfFREIHwgVEcgXSAgTlVNQkVSOwpjb21tYW5kIDogWyBCQyB8IExDXTsKY29tbWFuZCA6IFtDTEVBTiB8IEhPTUVdOwoKCgpyZXBlYXQgICAgICAgICA6IFJFUEVBVCBOVU1CRVIgIlsiIGluc3RydWN0aW9uKiAiXSIgOwoKcHJvY2VkdXJlX2RlZmluaXRpb24gOiBQTyBJRCBwYXJhbWV0ZXIqIGluc3RydWN0aW9uKiBFTkQ7Cgpwcm9jZWR1cmVfY2FsbCA6IElEIGV4cHJlc3Npb24qICA7CgpwYXJhbWV0ZXIgICAgOiBDT0xPTiBJRCA7CiNleHByZXNzaW9uIDogW2V4cHJlc3Npb25fbnVtYmVyIHwgZXhwcmVzc2lvbl9wYXJhbWV0ZXJdOwpleHByZXNzaW9uICA6IE5VTUJFUjsKZXhwcmVzc2lvbiA6IHBhcmFtZXRlciA7JCMkQVYgMTA=)
