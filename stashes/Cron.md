---
id: cda5130e-5c56-42dc-868a-793adf042da3
title: Cron
desc: a category to store note about a cron expression parser
updated: 1775820939
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