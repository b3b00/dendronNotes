---
id: i3uw0392gmevv89ykco8v9x
title: string-escaping-opt-in
desc: ''
updated: 1729682476437
created: 1729669890142
---


# do not escape strings if not needed (as an opt-in for compatibility)


## How

```c#
// escape : default behavior
[String(doEscape:true, channel:0)] STRING = 1,

// do not escape
[String(doEscape:false, channel:0)] STRING = 1,

```
PR [#492](https://github.com/b3b00/csly/pull/492)

## Benchmark

```

BenchmarkDotNet v0.13.12, Windows 10 (10.0.19045.5011/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.400
  [Host]     : .NET 7.0.20 (7.0.2024.26716), X64 RyuJIT AVX2
  DefaultJob : .NET 7.0.20 (7.0.2024.26716), X64 RyuJIT AVX2


```
| Method            | Mean     | Error   | StdDev   | Median   | Gen0       | Gen1      | Gen2      | Allocated |
|------------------ |---------:|--------:|---------:|---------:|-----------:|----------:|----------:|----------:|
| escaping | 140.2 ms | 5.03 ms | 14.44 ms | 135.5 ms | 10666.6667 | 3666.6667 | 1666.6667 |  62.12 MB |
| not escaping   | 131.9 ms | 4.73 ms | 13.19 ms | 129.5 ms |  9000.0000 | 3500.0000 | 1500.0000 |   56.1 MB |


# analysis

**CPU :**

`(135.5 - 129.5) / 12.95 = 0.04`

**-4%** 👍

**Memory :**

`(62.12 - 56.1) / 56.1 = 0.107`

**-10.7%** 👍


## benchmark #2

```

BenchmarkDotNet v0.13.12, Windows 10 (10.0.19045.5011/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.400
  [Host]     : .NET 7.0.20 (7.0.2024.26716), X64 RyuJIT AVX2
  DefaultJob : .NET 7.0.20 (7.0.2024.26716), X64 RyuJIT AVX2


```
| Method             | Mean     | Error   | StdDev  | Gen0       | Gen1      | Gen2      | Allocated |
|------------------- |---------:|--------:|--------:|-----------:|----------:|----------:|----------:|
| TestNotEscapedJson | 119.3 ms | 2.76 ms | 7.92 ms |  9600.0000 | 3400.0000 | 1400.0000 |   56.1 MB |
| TestEscapedJson    | 122.8 ms | 3.44 ms | 9.76 ms | 10600.0000 | 3400.0000 | 1400.0000 |  62.12 MB |

# analysis #2

** CPU : - 2 % ** 👍

** Memory : -28 % !** 👍👍