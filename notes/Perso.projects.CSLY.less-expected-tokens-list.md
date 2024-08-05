---
id: Perso.projects.CSLY.less-expected-tokens-list
title: Perso.projects.CSLY.less-expected-tokens-list
desc: Memory usage benchmark
updated: 0
created: 0
---
# trying to reduce memory

Profiling memory on SimpleExpressionParser, we find that we create a lot of `List<UnexpectedTokenSyntaxErorr<EpressionToken>>`. This can be explained by backtracking but since CSLY expression parsing try to limit backtracking there mays be room to better memory usage.

# benchmarking

With benchmark dot net we look at memory (and CPU) usage for a 50 `1+2+3+4+5+6+7+8+9+10+11+12+13+14+15+16+17+18+19+20` parse loop



## initial state (no optim at all)

## stopping completly `List<UnexpectedTokenSyntaxErorr<EpressionToken>>`

We remove completely the List creation ins SyntaxParseResult. Of course this is not desirable at it would give useless eror messages. But at least will give us a bottom line for optimizing the List creations.






