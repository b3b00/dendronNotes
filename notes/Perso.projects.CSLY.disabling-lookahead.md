---
id: Perso.projects.CSLY.disabling-lookahead
title: Perso.projects.CSLY.disabling-lookahead
desc: Dummy test : completly disable lookahead
updated: 1722431227029
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

| Method        | Memoize | Broaden | NoLookAheadAtAll | Mean        | Error     | StdDev     | Median      | Gen0        | Gen1        | Gen2       | Allocated  |
|-------------- |-------- |-------- |----------------- |------------:|----------:|-----------:|------------:|------------:|------------:|-----------:|-----------:|
| **TestBackTrack** | **False**   | **False**   | **False**            |   **975.75 ms** | **20.542 ms** |  **55.885 ms** |   **958.81 ms** | **225000.0000** |  **40000.0000** |  **8000.0000** | **1128.25 MB** |
| **TestBackTrack** | **False**   | **True**    | **False**            |   **976.68 ms** | **23.612 ms** |  **67.747 ms** |   **959.68 ms** | **223000.0000** |  **40000.0000** |  **8000.0000** | **1124.19 MB** |
| **TestBackTrack** | **True**    | **False**   | **False**            |    **56.25 ms** |  **1.279 ms** |   **3.629 ms** |    **55.37 ms** |   **5222.2222** |   **3111.1111** |  **2666.6667** |   **34.79 MB** |
| **TestBackTrack** | **True**    | **True**    | **False**            |    **62.25 ms** |  **3.098 ms** |   **8.988 ms** |    **60.68 ms** |   **5111.1111** |   **2888.8889** |  **2444.4444** |   **34.79 MB** |
| **TestBackTrack** | **False**   | **False**   | **True**             | **3,004.40 ms** | **59.379 ms** | **114.404 ms** | **2,982.34 ms** | **477000.0000** | **150000.0000** | **18000.0000** | **3113.86 MB** |
| **TestBackTrack** | **True**    | **True**    | **True**             |   **266.50 ms** | **35.118 ms** | **103.547 ms** |   **238.53 ms** |  **15000.0000** |   **5666.6667** |  **4333.3333** |  **137.89 MB** |
| **TestBackTrack** | **True**    | **False**   | **True**             |   **170.68 ms** |  **5.942 ms** |  **17.334 ms** |   **165.76 ms** |  **15000.0000** |   **5333.3333** |  **4333.3333** |  **137.89 MB** |
| **TestBackTrack** | **False**   | **True**    | **True**             | **3,020.54 ms** | **64.886 ms** | **179.798 ms** | **2,949.98 ms** | **477000.0000** | **149000.0000** | **19000.0000** | **3113.88 MB** |



**[Indented While](https://github.com/b3b00/csly/blob/dev/src/samples/IndentedWhile/parser/IndentedWhileParserGeneric.cs)**

| Method    | Memoize | Broaden | NoLookAheadAtAll | Mean     | Error    | StdDev   | Median   | Gen0    | Allocated |
|---------- |-------- |-------- |----------------- |---------:|---------:|---------:|---------:|--------:|----------:|
| **TestWhile** | **False**   | **False**   | **False**            | **45.75 μs** | **0.900 μs** | **1.937 μs** | **45.36 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **False**   | **True**    | **False**            | **47.87 μs** | **0.918 μs** | **2.589 μs** | **46.99 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **True**    | **False**   | **False**            | **45.85 μs** | **0.927 μs** | **2.599 μs** | **45.19 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **True**    | **True**    | **False**            | **47.55 μs** | **1.171 μs** | **3.302 μs** | **46.79 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **True**    | **True**    | **True**             | **45.48 μs** | **0.867 μs** | **2.224 μs** | **45.00 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **False**   | **False**   | **True**             | **48.58 μs** | **1.580 μs** | **4.481 μs** | **47.07 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **True**    | **False**   | **True**             | **48.41 μs** | **1.807 μs** | **5.126 μs** | **46.49 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **False**   | **True**    | **True**             | **50.99 μs** | **2.119 μs** | **5.941 μs** | **48.99 μs** | **22.2778** |  **91.17 KB** |

These times are senseless. a new bench is needed.


| Method    | Memoize | Broaden | NoLookAheadAtAll | Mean     | Error    | StdDev    | Median   | Gen0    | Allocated |
|---------- |-------- |-------- |----------------- |---------:|---------:|----------:|---------:|--------:|----------:|
| **TestWhile** | **False**   | **False**   | **False**            | **43.81 μs** | **0.866 μs** |  **1.185 μs** | **43.64 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **True**    | **True**    | **False**            | **50.80 μs** | **1.319 μs** |  **3.632 μs** | **49.58 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **False**   | **True**    | **False**            | **51.09 μs** | **1.515 μs** |  **4.322 μs** | **49.07 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **True**    | **False**   | **False**            | **53.16 μs** | **2.014 μs** |  **5.648 μs** | **51.03 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **True**    | **False**   | **True**             | **56.86 μs** | **2.483 μs** |  **6.714 μs** | **54.89 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **False**   | **True**    | **True**             | **48.76 μs** | **0.868 μs** |  **0.678 μs** | **48.96 μs** | **22.2168** |  **91.17 KB** |
| **TestWhile** | **True**    | **True**    | **True**             | **58.80 μs** | **3.985 μs** | **11.175 μs** | **55.87 μs** | **22.2778** |  **91.17 KB** |
| **TestWhile** | **False**   | **False**   | **True**             | **49.65 μs** | **2.903 μs** |  **7.948 μs** | **47.01 μs** | **22.2778** |  **91.17 KB** |
