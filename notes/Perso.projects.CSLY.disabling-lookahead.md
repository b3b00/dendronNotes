---
id: Perso.projects.CSLY.disabling-lookahead
title: Perso.projects.CSLY.disabling-lookahead
desc: Dummy test : completly disable lookahead
updated: 1722424556185
created: 0
---
# This tests how much performance degrade when removing lookahead (1).

```

BenchmarkDotNet v0.13.12, Windows 10 (10.0.19045.4651/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.107
  [Host]     : .NET 7.0.4 (7.0.423.11508), X64 RyuJIT AVX2
  DefaultJob : .NET 7.0.4 (7.0.423.11508), X64 RyuJIT AVX2


```
| Method    | NoLookAheadAtAll | Mean      | Error    | StdDev    | Median    | Gen0       | Gen1      | Gen2      | Allocated |
|---------- |----------------- |----------:|---------:|----------:|----------:|-----------:|----------:|----------:|----------:|
| **TestDummy** | **False**            |  **55.26 ms** | **1.491 ms** |  **4.253 ms** |  **54.85 ms** |  **5333.3333** | **3222.2222** | **2555.5556** |  **34.79 MB** |
| **TestDummy** | **True**             | **169.69 ms** | **4.829 ms** | **13.698 ms** | **165.66 ms** | **14000.0000** | **5000.0000** | **3333.3333** | **137.89 MB** |


Strangely disbaling lookahead (and then trying every single rule for each non terminal) gives us a much better performance. 

Let's see how it compounds with memoization optimization.
