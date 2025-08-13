---
id: Perso.projects.console-interactive
title: Perso.projects.console-interactive
desc: Interactive Console
updated: 1755094999868
created: 0
---
# What

A nuget to easily create interactive console application.

See [spectre console prompts](https://spectreconsole.net/prompts/)

**primitives for inputs**

* [ ] single  typed inputs
* [ ] Password
* [x] select
* [ ] autocomplete ?
* [ ] with pattern ? : think date `_ _ / _ _ / _ _ _ _`
* [ ] Form objects (opt)

Automatically generate a form (sequence of inputs) from a class:

```
public class MyForm {
    [Input(0, "Wath's your name ?")]
    public string Name { get; set; }
    
    [Input(1, "How old are you ?")]
    public int Age  { get; set; }
    
    [Input(2, "Are you happy ?")]
    public bool Agree { get; set; }
}
```

this will ask successively for name , age and happyness.

**Validations**

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

* enter the auto generated chaise number
* enter a part of the string
  * if the entrez part le nom full discriminant the filières list is redisplayed

