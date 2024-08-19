---
id: Perso.projects.CSLY.benchmarking-memory
title: becnhmarking-memory
desc: Memory usage benchmark
updated: 1723191532058
created: 0
---
# trying to reduce memory

Profiling memory on SimpleExpressionParser, we find that we create a lot of `List<UnexpectedTokenSyntaxErorr<EpressionToken>>`. This can be explained by backtracking but since CSLY expression parsing try to limit backtracking there may be room to better memory usage.

# benchmarking

With benchmark dot net we look at memory (and CPU) usage for a 50 `1+2+3+4+5+6+7+8+9+10+11+12+13+14+15+16+17+18+19+20` parse loop

| Method        | Mean     | Error    | StdDev   | Gen0      | Gen1    | Allocated |
|-------------- |---------:|---------:|---------:|----------:|--------:|----------:|
| - | 14.91 ms | 1.869 ms | 5.512 ms | 4109.3750 | 15.6250 |  16.42 MB |





## do not instanciate when not an error

DotMemory gives us : 771.8 KB allocated for `List<UnexpectedTokenSyntaxErorr<EpressionToken>>`

A List is systematically created on ParseResult which is not usefull if the result is a success. So remove this useless instanciation.


| Method        | Mean     | Error     | StdDev    | Gen0      | Gen1    | Allocated |
|-------------- |---------:|----------:|----------:|----------:|--------:|----------:|
| - | 8.874 ms | 0.2074 ms | 0.5780 ms | 4078.1250 | 15.6250 |  16.28 MB |


Time is much better ! 
I would have expected lower CPU boost but greater memory gain 😊
But we now have 6 failing unit tests.


## limit lambdas on `Rule.Match` 

| Method        | Mean     | Error     | StdDev   | Median   | Gen0      | Allocated |
|-------------- |---------:|----------:|---------:|---------:|----------:|----------:|
| - | 9.752 ms | 0.3932 ms | 1.135 ms | 9.262 ms | 3750.0000 |  14.99 MB |

Memory allocation is better but time is a little bit worse 😞

let's cross check : 

| Method        | Mean     | Error     | StdDev    | Gen0      | Allocated |
|-------------- |---------:|----------:|----------:|----------:|----------:|
| TestBackTrack | 7.892 ms | 0.1503 ms | 0.1671 ms | 3750.0000 |  14.99 MB |

way better 😊

## LeadingTokens

Pool LeadingToken for <T> => only one LeadingToken by enum value.

| Method        | Mean     | Error     | StdDev    | Gen0      | Allocated |
|-------------- |---------:|----------:|----------:|----------:|----------:|
| TestBackTrack | 8.602 ms | 0.1398 ms | 0.1308 ms | 3750.0000 |  14.99 MB |

not that concluding ....



### ExpectingTokens on ParseResult

⚠️ error messages are not really good !

| Method        | Mean     | Error     | StdDev    | Gen0      | Gen1   | Allocated |
|-------------- |---------:|----------:|----------:|----------:|-------:|----------:|
| TestBackTrack | 7.891 ms | 0.2268 ms | 0.6284 ms | 3500.0000 | 7.8125 |  13.99 MB |

and without  leadingtoken pool : 

| Method        | Mean     | Error     | StdDev    | Gen0      | Gen1   | Allocated |
|-------------- |---------:|----------:|----------:|----------:|-------:|----------:|
| TestBackTrack | 7.698 ms | 0.1232 ms | 0.0962 ms | 3500.0000 | 7.8125 |  13.99 MB |

### using generic parser instead 
| Method                | Mean     | Error     | StdDev    | Gen0      | Gen1   | Allocated |
|---------------------- |---------:|----------:|----------:|----------:|--------|-----------:|
| TestExpressionGeneric | 6.776 ms | 0.1064 ms | 0.0996 ms | 3750.0000 |        |  15.01 MB |
| TestExpressionRegex   | 6.618 ms | 0.1220 ms | 0.1019 ms | 3500.0000 | 7.8125 |  13.99 MB |

the CPU time difference is not significant.... may be .Net 8 brings enough performance boost to now make regex lexer good enough ?

** 2nd run (splitted) **

| Method              | Mean     | Error     | StdDev   | Median | Gen0      | Gen1    | Allocated |
|-------------------- |---------:|----------:|---------:|---:|-------:|--------:|----------:|
| TestExpressionRegex | 8.903 ms | 0.4127 ms | 1.197 ms | | 3500.0000 | 15.6250 |  13.99 MB |
| TestExpressionGeneric | 9.557 ms | 0.5160 ms | 1.480 ms | 9.080 ms | 3750.0000 | | 15.01 MB |



** 3rd run **
```
BenchmarkDotNet v0.13.12, Windows 10 (10.0.19045.4651/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.107
  [Host]     : .NET 7.0.4 (7.0.423.11508), X64 RyuJIT AVX2
  DefaultJob : .NET 7.0.4 (7.0.423.11508), X64 RyuJIT AVX2
```

| Method                | Mean      | Error     | StdDev    | Median   | Gen0      | Gen1    | Allocated |
|---------------------- |----------:|----------:|----------:|---------:|----------:|--------:|----------:|
| TestExpressionRegex   | 10.043 ms | 0.7881 ms | 2.3114 ms | 8.946 ms | 3500.0000 | 15.6250 |  13.99 MB |
| TestExpressionGeneric |  8.394 ms | 0.1660 ms | 0.4460 ms | 8.361 ms | 3750.0000 |       - |  15.01 MB |

| Method                | Mean     | Error     | StdDev    | Median   | Gen0      | Gen1    | Allocated |
|---------------------- |---------:|----------:|----------:|---------:|----------:|--------:|----------:|
| TestExpressionRegex   | 8.137 ms | 0.1622 ms | 0.4009 ms | 8.119 ms | 3500.0000 | 15.6250 |  13.99 MB |
| TestExpressionGeneric | 8.599 ms | 0.1848 ms | 0.5242 ms | 8.382 ms | 3757.8125 |       - |  15.01 MB |

It looks like genericLexer instantiates more Gen0 object. This needs to be investigated.

| Method                | Mean     | Error    | StdDev   | Median   | Gen0      | Gen1    | Allocated |
|---------------------- |---------:|---------:|---------:|---------:|----------:|--------:|----------:|
| TestExpressionRegex   | 10.14 ms | 0.444 ms | 1.258 ms | 9.767 ms | 3500.0000 | 15.6250 |  13.99 MB |
| TestExpressionGeneric | 10.11 ms | 0.423 ms | 1.234 ms | 9.635 ms | 3750.0000 |       - |  15.01 MB |