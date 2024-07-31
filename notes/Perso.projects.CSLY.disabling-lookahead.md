---
id: Perso.projects.CSLY.disabling-lookahead
title: Perso.projects.CSLY.disabling-lookahead
desc: Dummy test : completly disable lookahead
updated: 1722427605310
created: 0
---
# This tests how much performance degrade when removing lookahead (1).


**Acknowledgement** : this is completly silly as it will lead to poor error messages.

## no lookahead solo

## backtrack parser

 - no lookahead
 - memoization on
 - broadentokenwindow = off

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

As expected disabling lookahead leads to a dramatic performance drop : **3x** slower and **4x** allocated memory. 



# Memoization and lookahead

**[backtracking parser](https://github.com/b3b00/csly/blob/dev/src/benchCurrent/backtrack/BackTrackParser.cs)**


| Method    | Memoize | NoLookAheadAtAll | Mean      | Error    | StdDev    | Gen0       | Gen1      | Gen2      | Allocated |
|---------- |-------- |----------------- |----------:|---------:|----------:|-----------:|----------:|----------:|----------:|
| **TestDummy** | **False**   | **False**            |  **54.29 ms** | **1.149 ms** |  **3.335 ms** |  **5333.3333** | **3222.2222** | **2555.5556** |  **34.79 MB** |
| **TestDummy** | **True**    | **False**            |  **57.78 ms** | **1.627 ms** |  **4.668 ms** |  **5333.3333** | **3111.1111** | **2555.5556** |  **34.79 MB** |
| **TestDummy** | **False**   | **True**             | **161.32 ms** | **3.216 ms** |  **8.585 ms** | **15000.0000** | **5666.6667** | **4333.3333** | **137.89 MB** |
| **TestDummy** | **True**    | **True**             | **164.81 ms** | **4.693 ms** | **13.616 ms** | **14000.0000** | **4666.6667** | **3333.3333** | **137.89 MB** |

We can see that memoization does not bring any performance boost when 

**[json parser](https://github.com/b3b00/csly/blob/dev/src/benchCurrent/json/EbnfJsonGenericParser.cs)**

| Method   | Memoize | NoLookAheadAtAll | Mean     | Error    | StdDev   | Median   | Gen0       | Gen1       | Gen2      | Allocated |
|--------- |-------- |----------------- |---------:|---------:|---------:|---------:|-----------:|-----------:|----------:|----------:|
| **TestJson** | **False**   | **False**            | **110.1 ms** |  **2.16 ms** |  **5.97 ms** | **108.4 ms** | **11200.0000** |  **3600.0000** | **1600.0000** |  **65.79 MB** |
| **TestJson** | **False**   | **True**             | **275.3 ms** |  **6.72 ms** | **19.38 ms** | **268.5 ms** | **23000.0000** |  **9000.0000** | **4000.0000** | **135.61 MB** |
| **TestJson** | **True**    | **False**            | **154.5 ms** |  **3.73 ms** | **10.71 ms** | **153.1 ms** | **13333.3333** |  **5333.3333** | **2666.6667** |  **78.41 MB** |
| **TestJson** | **True**    | **True**             | **468.0 ms** | **14.33 ms** | **40.41 ms** | **457.6 ms** | **27000.0000** | **10000.0000** | **4000.0000** | **161.52 MB** |

We can see the [#435](https://github.com/b3b00/csly/issues/435) issue here where memoization is not a good thing for json parser.
Look ahead is still useful for parsing json JSON even though the performance drop is lower than for backtracking parser ( **2.5x** CPU and **2x** memory)

# The whole thing

**[json parser](https://github.com/b3b00/csly/blob/dev/src/benchCurrent/json/EbnfJsonGenericParser.cs)**

| Method   | Memoize | NoLookAheadAtAll | Mean     | Error    | StdDev   | Median   | Gen0       | Gen1       | Gen2      | Allocated |
|--------- |-------- |----------------- |---------:|---------:|---------:|---------:|-----------:|-----------:|----------:|----------:|
| **TestJson** | **False**   | **False**            | **119.2 ms** |  **3.55 ms** | **10.08 ms** | **116.2 ms** | **11200.0000** |  **3600.0000** | **1600.0000** |  **65.79 MB** |
| **TestJson** | **False**   | **True**             | **260.9 ms** |  **5.21 ms** |  **7.46 ms** | **261.4 ms** | **23000.0000** |  **9000.0000** | **4000.0000** | **135.61 MB** |
| **TestJson** | **True**    | **False**            | **152.5 ms** |  **3.34 ms** |  **9.48 ms** | **150.3 ms** | **13666.6667** |  **5666.6667** | **2666.6667** |  **78.41 MB** |
| **TestJson** | **True**    | **True**             | **470.5 ms** | **11.92 ms** | **34.39 ms** | **464.7 ms** | **27000.0000** | **10000.0000** | **4000.0000** | **161.52 MB** |

**[backtracking parser](https://github.com/b3b00/csly/blob/dev/src/benchCurrent/backtrack/BackTrackParser.cs)**




**[Indented While](https://github.com/b3b00/csly/blob/dev/src/samples/IndentedWhile/parser/IndentedWhileParserGeneric.cs)**

