---
id: Perso.projects.CSLY.perfs.bench-csly-variants.json
title: Perso.projects.CSLY.perfs.bench-csly-variants.json
desc: JSON benchs
updated: 1779309228459
created: 0
---
| Method        | Type | Mean           | Error         | StdDev        | Median         | Gen0     | Gen1     | Allocated  |
|-------------- |----- |---------------:|--------------:|--------------:|---------------:|---------:|---------:|-----------:|
| **TestCsly**      | **Big**  | **3,633,735.7 ns** | **131,051.04 ns** | **365,317.95 ns** | **3,503,082.2 ns** | **675.7813** | **582.0313** | **8291.02 KB** |
| TestFluent    | Big  |   807,092.1 ns |  21,753.09 ns |  64,139.49 ns |   798,696.5 ns | 226.5625 | 148.4375 | 2844.23 KB |
| TestGenerated | Big  |     1,318.2 ns |      59.94 ns |     176.72 ns |     1,377.7 ns |   1.7223 |   0.1221 |   21.15 KB |
| |  | | | | | | | |
| **TestCsly**      | **Long** | **2,210,593.8 ns** |  **29,763.37 ns** |  **26,384.45 ns** | **2,214,427.3 ns** | **617.1875** | **515.6250** | **7587.14 KB** |
| TestFluent    | Long |   462,387.3 ns |  17,913.36 ns |  51,107.84 ns |   443,332.3 ns | 174.8047 | 121.0938 | 2150.48 KB |
| TestGenerated | Long |     1,125.4 ns |      22.23 ns |      56.18 ns |     1,117.0 ns |   1.2293 |   0.0610 |   15.08 KB |
| |  | | | | | | | |
| **TestCsly**      | **Wide** | **1,193,454.8 ns** |  **23,562.85 ns** |  **55,077.41 ns** | **1,199,193.0 ns** | **273.4375** | **171.8750** | **3376.03 KB** |
| TestFluent    | Wide |   389,240.2 ns |  25,535.30 ns |  72,439.39 ns |   353,313.3 ns | 126.9531 |  73.7305 | 1558.47 KB |
| TestGenerated | Wide |       792.2 ns |      15.81 ns |      38.48 ns |       788.2 ns |   1.1501 |   0.0515 |   14.08 KB |
| |  | | | | | | | |
| **TestCsly**      | **Deep** |             **NA** |            **NA** |            **NA** |             **NA** |       **NA** |       **NA** |         **NA** |
| TestFluent    | Deep |   275,084.4 ns |   2,916.92 ns |   2,435.76 ns |   275,155.6 ns | 121.0938 |  62.9883 |  1487.5 KB |
| TestGenerated | Deep |       874.7 ns |      78.01 ns |     227.56 ns |       798.8 ns |   0.8650 |   0.0296 |   10.59 KB |

Fluent parser is consistently better, except onwide json....

## fluently build classical csly

```

BenchmarkDotNet v0.15.8, Windows 11 (10.0.26100.8246/24H2/2024Update/HudsonValley)
Intel Core Ultra 7 265H 2.20GHz, 1 CPU, 16 logical and 16 physical cores
.NET SDK 10.0.300
  [Host]     : .NET 10.0.8 (10.0.8, 10.0.826.23019), X64 RyuJIT x86-64-v3
  DefaultJob : .NET 10.0.8 (10.0.8, 10.0.826.23019), X64 RyuJIT x86-64-v3


```
| Method         | Type | Mean           | Error         | StdDev        | Median         | Gen0     | Gen1     | Allocated  |
|--------------- |----- |---------------:|--------------:|--------------:|---------------:|---------:|---------:|-----------:|
| **TestCsly**       | **Big**  | **2,847,032.6 ns** | **107,025.97 ns** | **307,077.59 ns** | **2,754,156.2 ns** | **656.2500** | **328.1250** | **8155.04 KB** |
| TestCslyFluent | Big  | 2,830,606.3 ns |  69,000.41 ns | 196,862.06 ns | 2,767,232.8 ns | 675.7813 | 578.1250 | 8321.31 KB |
| TestFluent     | Big  |   629,490.2 ns |  12,836.26 ns |  36,622.56 ns |   617,416.8 ns | 231.4453 | 159.1797 |  2844.1 KB |
| TestGenerated  | Big  |     1,053.1 ns |      20.44 ns |      18.12 ns |     1,052.2 ns |   1.7223 |   0.1221 |   21.15 KB |
| **TestCsly**       | **Long** | **2,135,611.7 ns** |  **30,540.66 ns** |  **27,073.50 ns** | **2,131,010.4 ns** | **617.1875** | **515.6250** |  **7577.1 KB** |
| TestCslyFluent | Long | 2,092,735.7 ns |  38,798.19 ns |  47,647.66 ns | 2,091,487.9 ns | 621.0938 | 519.5313 | 7629.22 KB |
| TestFluent     | Long |   424,396.2 ns |   8,464.52 ns |   9,408.29 ns |   422,806.1 ns | 175.2930 | 122.0703 | 2150.48 KB |
| TestGenerated  | Long |       809.1 ns |      16.06 ns |      29.37 ns |       810.0 ns |   1.2293 |   0.0610 |   15.08 KB |
| **TestCsly**       | **Wide** |   **936,471.3 ns** |  **18,464.29 ns** |  **38,131.95 ns** |   **925,413.0 ns** | **275.3906** | **175.7813** |  **3375.9 KB** |
| TestCslyFluent | Wide |   906,074.2 ns |   9,514.55 ns |   8,434.40 ns |   907,172.9 ns | 276.3672 | 174.8047 |  3388.1 KB |
| TestFluent     | Wide |   307,807.4 ns |   6,140.99 ns |   7,985.03 ns |   305,942.7 ns | 126.9531 |  73.7305 | 1558.46 KB |
| TestGenerated  | Wide |       742.8 ns |       7.03 ns |       5.49 ns |       744.0 ns |   1.1501 |   0.0515 |   14.08 KB |
| **TestCsly**       | **Deep** |             **NA** |            **NA** |            **NA** |             **NA** |       **NA** |       **NA** |         **NA** |
| TestCslyFluent | Deep |             NA |            NA |            NA |             NA |       NA |       NA |         NA |
| TestFluent     | Deep |   264,715.9 ns |   3,476.61 ns |   2,714.31 ns |   263,772.8 ns | 121.0938 |  62.9883 |  1487.5 KB |
| TestGenerated  | Deep |       629.1 ns |      12.03 ns |      13.86 ns |       624.1 ns |   0.8650 |   0.0296 |   10.59 KB |

Benchmarks with issues:
  CsliesBench.TestCsly: DefaultJob [Type=Deep]
  CsliesBench.TestCslyFluent: DefaultJob [Type=Deep]


Building fluently from introspection is not a way.
