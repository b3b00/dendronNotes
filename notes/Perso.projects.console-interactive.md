---
id: Perso.projects.console-interactive
title: Perso.projects.console-interactive
desc: Interactive Console
updated: 1754724648062
created: 0
---
# What
A nuget to easily create interactive console application.

 **primitives for inputs**
- single  typed inputs
- select
- autocomplete ?

# How
```csharp
int i = AskSingle<int>("enter an integer);
String choice = AskSelect<string>("choose", "chose 1", "chose 2");
```

## selects 
```
Chose one : 
- 1 : chaise 1
- 2 : choice 2
:
```
User c an situer : 
- enter the auto generated chaise number
- enter a part of the string
   - if the entrez part le nom full discriminant the filières list is redisplayed

