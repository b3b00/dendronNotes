---
id: cda5130e-5c56-42dc-868a-793adf042da3
title: Cron
desc: a category to store note about a cron expression parser
updated: 1775820984
created: 1775820893
---

# CSLY grammar

```
genericLexer CronLexer;

[Int] INT;

[Sugar] STAR : "*" ;

[Sugar] SLASH : "/" ;

[Sugar] COMMA : "," ;

[Sugar] DASH : "-" ;

# days
[KeyWord] MONDAY : "MON";
[KeyWord] TUESDAY : "TUE";
[KeyWord] WEDNESDAY : "WED";
[KeyWord] THURSDAY : "THU";
[KeyWord] FRIDAY : "FRI";
[KeyWord] SATURDAY : "SAT";
[KeyWord] SUNDAY : "SUN";

# months

[KeyWord] JANUARY :"JAN";
[KeyWord] FEBRUARY :"FEB";
[KeyWord] MARCH :"MAR";
[KeyWord] APRIL :"APR";
[KeyWord] MAY :"MAY";
[KeyWord] JUNE :"JUN";
[KeyWord] JULY :"JUL";
[KeyWord] AUGUST :"AUG";
[KeyWord] SEPTEMBER :"SEP";
[KeyWord] OCTOBER:"OCT";
[KeyWord] NOVEMBER :"NOV";
[KeyWord] DECEMBER :"DEC";

parser CronParser;

-> cron-entry  : minute hour dom month dow ;

minute : field ;
hour  :  field ;
dom   :  field ;
month :  field ;
dow   :  field ;

field : [ star | field-list ] ;

star : STAR;

field-list : field-item ( field-item COMMA field-list )* ;

field-item  : [ value | range | step | range-step ];

range : value DASH value ;
step : STAR SLASH INT ;
range-step : range SLASH INT ;

value : INT; 
value : name ;


name : [ month-name | dow-name ] ;

month-name : [ JANUARY | FEBRUARY | MARCH | APRIL | MAY | JUNE | JULY | AUGUST | SEPTEMBER | OCTOBER | NOVEMBER | DECEMBER ];

dow-name : [ MONDAY | TUESDAY | WEDNESDAY | THURSDAY | FRIDAY | SATURDAY | SUNDAY ];



```
______________

# CSLY viz

[Cron expression parser](https://b3b00.github.io/CslyViz/#Q1NMWS1NQVJLI2dlbmVyaWNMZXhlciBDcm9uTGV4ZXI7CgpbSW50XSBJTlQ7CgpbU3VnYXJdIFNUQVIgOiAiKiIgOwoKW1N1Z2FyXSBTTEFTSCA6ICIvIiA7CgpbU3VnYXJdIENPTU1BIDogIiwiIDsKCltTdWdhcl0gREFTSCA6ICItIiA7CgojIGRheXMKW0tleVdvcmRdIE1PTkRBWSA6ICJNT04iOwpbS2V5V29yZF0gVFVFU0RBWSA6ICJUVUUiOwpbS2V5V29yZF0gV0VETkVTREFZIDogIldFRCI7CltLZXlXb3JkXSBUSFVSU0RBWSA6ICJUSFUiOwpbS2V5V29yZF0gRlJJREFZIDogIkZSSSI7CltLZXlXb3JkXSBTQVRVUkRBWSA6ICJTQVQiOwpbS2V5V29yZF0gU1VOREFZIDogIlNVTiI7CgojIG1vbnRocwoKW0tleVdvcmRdIEpBTlVBUlkgOiJKQU4iOwpbS2V5V29yZF0gRkVCUlVBUlkgOiJGRUIiOwpbS2V5V29yZF0gTUFSQ0ggOiJNQVIiOwpbS2V5V29yZF0gQVBSSUwgOiJBUFIiOwpbS2V5V29yZF0gTUFZIDoiTUFZIjsKW0tleVdvcmRdIEpVTkUgOiJKVU4iOwpbS2V5V29yZF0gSlVMWSA6IkpVTCI7CltLZXlXb3JkXSBBVUdVU1QgOiJBVUciOwpbS2V5V29yZF0gU0VQVEVNQkVSIDoiU0VQIjsKW0tleVdvcmRdIE9DVE9CRVI6Ik9DVCI7CltLZXlXb3JkXSBOT1ZFTUJFUiA6Ik5PViI7CltLZXlXb3JkXSBERUNFTUJFUiA6IkRFQyI7CgpwYXJzZXIgQ3JvblBhcnNlcjsKCi0+IGNyb24tZW50cnkgIDogbWludXRlIGhvdXIgZG9tIG1vbnRoIGRvdyA7CgptaW51dGUgOiBmaWVsZCA7CmhvdXIgIDogIGZpZWxkIDsKZG9tICAgOiAgZmllbGQgOwptb250aCA6ICBmaWVsZCA7CmRvdyAgIDogIGZpZWxkIDsKCmZpZWxkIDogWyBzdGFyIHwgZmllbGQtbGlzdCBdIDsKCnN0YXIgOiBTVEFSOwoKZmllbGQtbGlzdCA6IGZpZWxkLWl0ZW0gKCBmaWVsZC1pdGVtIENPTU1BIGZpZWxkLWxpc3QgKSogOwoKZmllbGQtaXRlbSAgOiBbIHZhbHVlIHwgcmFuZ2UgfCBzdGVwIHwgcmFuZ2Utc3RlcCBdOwoKcmFuZ2UgOiB2YWx1ZSBEQVNIIHZhbHVlIDsKc3RlcCA6IFNUQVIgU0xBU0ggSU5UIDsKcmFuZ2Utc3RlcCA6IHJhbmdlIFNMQVNIIElOVCA7Cgp2YWx1ZSA6IElOVDsgCnZhbHVlIDogbmFtZSA7CgoKbmFtZSA6IFsgbW9udGgtbmFtZSB8IGRvdy1uYW1lIF0gOwoKbW9udGgtbmFtZSA6IFsgSkFOVUFSWSB8IEZFQlJVQVJZIHwgTUFSQ0ggfCBBUFJJTCB8IE1BWSB8IEpVTkUgfCBKVUxZIHwgQVVHVVNUIHwgU0VQVEVNQkVSIHwgT0NUT0JFUiB8IE5PVkVNQkVSIHwgREVDRU1CRVIgXTsKCmRvdy1uYW1lIDogWyBNT05EQVkgfCBUVUVTREFZIHwgV0VETkVTREFZIHwgVEhVUlNEQVkgfCBGUklEQVkgfCBTQVRVUkRBWSB8IFNVTkRBWSBdOwokIyQ=)