---
id: lvsd5uo6f38wpi72a3ppssu
title: Perso.projects.CSLY.perfs.indentation-strings
desc: indentation string[] allocation
updated: 1729687369176
created: 1729665988221
---


# limit indentation string[] allocation


PR [#491](https://github.com/b3b00/csly/pull/491)

## baseline

```

BenchmarkDotNet v0.13.12, Windows 10 (10.0.19045.5011/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.400
  [Host]     : .NET 7.0.20 (7.0.2024.26716), X64 RyuJIT AVX2
  DefaultJob : .NET 7.0.20 (7.0.2024.26716), X64 RyuJIT AVX2


```
| Method   | Mean     | Error   | StdDev   | Gen0       | Gen1      | Gen2      | Allocated |
|--------- |---------:|--------:|---------:|-----------:|----------:|----------:|----------:|
| TestJson | 154.7 ms | 6.83 ms | 19.03 ms | 10500.0000 | 3750.0000 | 1750.0000 |  59.69 MB |



## limit string[] allocations 

```

BenchmarkDotNet v0.13.12, Windows 10 (10.0.19045.5011/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.400
  [Host]     : .NET 7.0.20 (7.0.2024.26716), X64 RyuJIT AVX2
  DefaultJob : .NET 7.0.20 (7.0.2024.26716), X64 RyuJIT AVX2


```
| Method   | Mean     | Error   | StdDev  | Gen0      | Gen1      | Gen2      | Allocated |
|--------- |---------:|--------:|--------:|----------:|----------:|----------:|----------:|
| TestJson | 111.4 ms | 2.63 ms | 7.34 ms | 9000.0000 | 3600.0000 | 1400.0000 |  53.16 MB |



# analysis

**CPU : - 27.9 %** 👍

 **Memory : - 10.9 %** 👍
