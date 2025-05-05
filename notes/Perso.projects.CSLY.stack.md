---
id: Perso.projects.CSLY.stack
title: Perso.projects.CSLY.stack
desc: stack parser
updated: 1746462080878
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
| Method               | parserType           | Mean      | Error     | StdDev    | Median    | Gen0       | Gen1       | Gen2       | Allocated |
|--------------------- |--------------------- |----------:|----------:|----------:|----------:|-----------:|-----------:|-----------:|----------:|
| **BenchLargeExpression** | **LL_RECURSIVE_DESCENT** | **440.52 ms** | **11.314 ms** | **32.280 ms** | **437.44 ms** | **86000.0000** | **32000.0000** | **20000.0000** | **464.21 MB** |
| **BenchLargeExpression** | **LL_STACK**             |  **19.81 ms** |  **0.693 ms** |  **1.931 ms** |  **19.25 ms** | **16500.0000** |   **968.7500** |          **-** |  **67.27 MB** |


Memory allocation is better as expected.
CPU usage is a real good surprise.
