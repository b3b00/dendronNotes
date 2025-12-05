---
id: 5lgec6henmp4n805pwla8mq
title: Perso.projects.CSLY.generator.working-notes
desc: working notes
updated: 1764950309588
created: 1764941289531
---

# While

## prefix visitors

For prefix 100 ( - not) operand and operators are swapped.

## extraneous dispatches

dispatches are generated for expression rules : 

```csharp
case "Expr_Prec_10_0":
                    return VisitExpr_Prec_10(node);
// Visitors for Expr_Prec_50
case "Expr_Prec_50_0":
                    return VisitExpr_Prec_50_0(node);
// Visitors for Expr_Prec_100
case "Expr_Prec_100_0":
                    return VisitExpr_Prec_100_0(node);
case "Expr_Prec_100_1":
                    return VisitExpr_Prec_100_1(node);
// Visitors for Expr_Operand
case "Expr_Operand_0":
                    return VisitExpr_Operand_0(node);
```

There should not be suffixes `_<INDEX>`.

In there MUST be `_<INDEX>` but they were not included for expressions and bypass rules.

## parse fails for factorial program

```
(
    r:=1;
    i:=1;
    while (i < 11 and true) do 
    ( 
      r := r * i;
      print r;
      print i;
      i := i + 1 
    )
)
```

Parse fails with `unepected SEMICOLON [;] @line 2, column 8 on channel 0. Expecting RPAREN.`





