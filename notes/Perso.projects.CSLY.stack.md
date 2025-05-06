---
id: Perso.projects.CSLY.stack
title: Perso.projects.CSLY.stack
desc: stack parser
<<<<<<< HEAD
updated: 1746521946369
||||||| parent of f4c87eb (.)
updated: 1746463168008
=======
updated: 1746463533943
>>>>>>> f4c87eb (.)
created: 0
---
# First bench (still work in progress).

## parser

[ExpressionParser](https://github.com/b3b00/csly/blob/experiment/stask-based-parser/src/samples/expressionParser/ExpressionParser.cs) : basic expression parser not using CSLY expression parsing mechanism.

## dataset

a random 1000 operands expression 

##results
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

Stack based parser can handler 10.000+ operands expressions. Recursivity hits tack overflow for ~ 4500 operands.


![](/assets/images/2025-05-05-18-45-20.png)

