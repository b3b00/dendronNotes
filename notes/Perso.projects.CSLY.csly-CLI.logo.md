---
id: Perso.projects.CSLY.csly-CLI.logo
title: Perso.projects.CSLY.csly-CLI.logo
desc: logo parser
updated: 0
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
