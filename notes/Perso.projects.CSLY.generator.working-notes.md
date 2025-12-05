---
id: 5lgec6henmp4n805pwla8mq
title: working-notes
desc: ''
updated: 1764941638610
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

There should not be suffixes _<INDEX>.





