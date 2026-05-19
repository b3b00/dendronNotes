---
id: Perso.projects.CSLY.perfs.bench-csly-variants.json
title: Perso.projects.CSLY.perfs.bench-csly-variants.json
desc: JSON benchs
updated: 1779216842073
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
