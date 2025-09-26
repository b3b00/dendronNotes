---
id: Perso.projects.CSLY.relaxed.visitor.typing
title: Perso.projects.CSLY.relaxed.visitor.typing
desc: Relaxed visitor typing
updated: 1758871283767
created: 0
---
# Goal
Allow relaxed visitor typing.
Actually visitor methods must be strongly typed with an unique return type.
Adding a more relaxed typing may help construct the visitor.


[issue #314](https://github.com/b3b00/csly/issues/314)

The parser type should still reflect the final result of a parse , i.e. the return type of the root rule is the output type. Parser.Parse then always returns the expected type and no cast is needed.



# Pain points

## Build

### EBNF : Groups / sub rules 

Groups generate hidden non terminals and rules. These use  

## Run

### EBNF specials

EBNF special type should be subtyped according to corresponding visitor's parameter type

**many rules (0+, 1+, {n,m})**

```csharp
[Production("grouping : intThing stringThing*")]
public GroupThing Grouping(int thing, List<string> things)
{
    return new GroupThing()
    {
        first = thing,
        others = things,
    };
}
```

**Sub rules / groups**

**Options**

```csharp
[Production("grouping : stringThing?")]
public string Grouping(ValueOption<string> optionStringThing)
{
    return optionStringThing.Match(
        (some) => some,
        () => "EMPTY");
}
```
