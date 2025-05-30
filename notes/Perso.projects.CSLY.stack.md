---
id: Perso.projects.CSLY.stack
title: Perso.projects.CSLY.stack
desc: stack parser
updated: 1746629619447
created: 0
---
# First bench (still work in progress).

## parser

[ExpressionParser](https://github.com/b3b00/csly/blob/experiment/stask-based-parser/src/samples/expressionParser/ExpressionParser.cs) : basic expression parser (BNF) not using CSLY expression parsing mechanism.

## dataset

a random 1000 operands expression 

## results
```

BenchmarkDotNet v0.13.12, Windows 10 (10.0.19045.5737/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.400
  [Host]     : .NET 8.0.15 (8.0.1525.16413), X64 RyuJIT AVX2
  DefaultJob : .NET 8.0.15 (8.0.1525.16413), X64 RyuJIT AVX2


```
| Method    | Mean      | Error     | StdDev    | Ratio         | RatioSD | Gen0       | Gen1       | Gen2       | Allocated | Alloc Ratio |
|---------- |----------:|----------:|----------:|--------------:|--------:|-----------:|-----------:|-----------:|----------:|------------:|
| recursive | 487.05 ms | 14.350 ms | 39.764 ms |      baseline |         | 86000.0000 | 34000.0000 | 20000.0000 | 461.95 MB |             |
| stacked   |  22.54 ms |  0.558 ms |  1.584 ms | 21.71x faster |   2.23x | 16562.5000 |   625.0000 |   156.2500 |  67.72 MB |  6.82x less |


 - Memory allocation is better as expected.
 - CPU usage is a real good surprise.

## results after more work (EBNF rule parser is now ok)

```

BenchmarkDotNet v0.13.12, Windows 10 (10.0.19045.5737/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.400
  [Host]     : .NET 8.0.15 (8.0.1525.16413), X64 RyuJIT AVX2
  DefaultJob : .NET 8.0.15 (8.0.1525.16413), X64 RyuJIT AVX2


```
| Method    | Mean      | Error     | StdDev    | Median    | Ratio        | RatioSD | Gen0       | Gen1       | Gen2       | Allocated | Alloc Ratio |
|---------- |----------:|----------:|----------:|----------:|-------------:|--------:|-----------:|-----------:|-----------:|----------:|------------:|
| recursive | 493.21 ms | 24.344 ms | 65.816 ms | 473.13 ms |     baseline |         | 87000.0000 | 34000.0000 | 21000.0000 | 468.39 MB |             |
| stacked   |  73.15 ms |  2.317 ms |  6.796 ms |  70.28 ms | 6.81x faster |   1.06x | 30666.6667 |   333.3333 |          - | 129.15 MB |  3.63x less |

The CPU boost has lowered, still 6x is a very good result.

# comparison
When runing both parsers against a growing expression (from 2 to 10000+ operands), stack based parser can handle easily 10.000+ operands expressions. Recursivity parser crash at ~4500 operands hitting a stack overflow.



![](/assets/images/2025-05-05-18-45-20.png)

