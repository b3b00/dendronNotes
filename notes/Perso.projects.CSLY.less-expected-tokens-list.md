---
id: Perso.projects.CSLY.less-expected-tokens-list
title: Perso.projects.CSLY.less-expected-tokens-list
desc: Memory usage benchmark
updated: 1722866144370
created: 0
---
# trying to reduce memory

Profiling memory on SimpleExpressionParser, we find that we create a lot of `List<UnexpectedTokenSyntaxErorr<EpressionToken>>`. This can be explained by backtracking but since CSLY expression parsing try to limit backtracking there mays be room to better memory usage.

# benchmarking

With benchmark dot net we look at memory (and CPU) usage for a 50 `1+2+3+4+5+6+7+8+9+10+11+12+13+14+15+16+17+18+19+20` parse loop

| Method        | Mean     | Error    | StdDev   | Gen0      | Gen1    | Allocated |
|-------------- |---------:|---------:|---------:|----------:|--------:|----------:|
| - | 14.91 ms | 1.869 ms | 5.512 ms | 4109.3750 | 15.6250 |  16.42 MB |


DotMemory gives us : 771.8 KB allocated for `List<UnexpectedTokenSyntaxErorr<EpressionToken>>`

## initial state (no optim at all)

## do not instanciate when not an error

A List si systematically created on ParseResult which is not usefull if the result is a success. So remove this senseless instanciation.


| Method        | Mean     | Error     | StdDev    | Gen0      | Gen1    | Allocated |
|-------------- |---------:|----------:|----------:|----------:|--------:|----------:|
| - | 8.874 ms | 0.2074 ms | 0.5780 ms | 4078.1250 | 15.6250 |  16.28 MB |


Time is much better ! 
I would have expected lower CPU boost but greater memory gain 😊
But we now have 6 failing unit tests.



