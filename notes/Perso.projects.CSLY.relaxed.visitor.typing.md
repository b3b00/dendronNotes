---
id: Perso.projects.CSLY.relaxed.visitor.typing
title: Perso.projects.CSLY.relaxed.visitor.typing
desc: Relaxed visitor typing
updated: 1759738044087
created: 0
---
# Goal
Allow relaxed visitor typing.
For now, visitor methods must be strongly typed with an unique return type.
Adding a more relaxed typing may help construct the visitor.


[issue #314](https://github.com/b3b00/csly/issues/314)

The parser could still reflect the final result of a parse , i.e. the return type of the root rule is the output type. Parser.Parse then always returns the expected type and no cast is needed.
But as we can call a parser.Parse with a starting rule this can not be ensured



# Pain points

## Build

### EBNF : Groups / sub rules 



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

**Sub rules / groups**

⚠️Groups can only contains non-terminals that return the exact same type !
because a `Group<IN,OUT>` has only one type for non terminal values.
