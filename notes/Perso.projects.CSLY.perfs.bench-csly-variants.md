---
id: Perso.projects.CSLY.perfs.bench-csly-variants
title: Perso.projects.CSLY.perfs.bench-csly-variants
desc: Bench CSLY variants
updated: 1779206851616
created: 0
---
#Goal

CSLY may be used in 3 different ways :
 1. the classical way using reflection
 2. the fluent way using the fluent API to build the parser
 3. the generative way, using csly.generator to build a static parser at build time

Goal is to bench all of them to find if any significative performance difference

## expected outcome

**fluent vs classical** : fluent built parser do not use instrospection to call visitor (instead they use direct lambda call with no type check), we can inspect that fluent parser is faster, and maybe more memory efficient ?.

generative vs fluent/classical : ?

## JSON Bench

using pidgin [bench json generator](https://github.com/benjamin-hodgson/Pidgin/blob/103dfc6d6a594310a6681def03bb7445f9a0cefc/Pidgin.Bench/JsonBench.cs#L154)

| Method        | Mean         | Error      | StdDev      | Median       | Gen0     | Gen1     | Allocated  |
|-------------- |-------------:|-----------:|------------:|-------------:|---------:|---------:|-----------:|
| TestCsly      | 3,074.527 us | 61.0952 us | 152.1483 us | 3,022.154 us | 671.8750 | 578.1250 | 8291.08 KB |
| TestFluent    |   678.133 us | 13.5435 us |  37.3028 us |   665.011 us | 231.4453 | 159.1797 | 2844.11 KB |
| TestGenerated |     1.145 us |  0.0229 us |   0.0522 us |     1.120 us |   1.7223 |   0.1221 |   21.15 KB |

As suspected fluent parser is faster and more memory efficient.
Generated parser is way more afficient (CPU and memory) !

__Idea for the future__ : we may be able to build a fluent parser using instrospection leading to best of both world (compactness, ease of writing, type safety and efficiency)

## Expressions Bench

on a source compound of 100 additions of 1 `1+1+1.....+1`

| Method        | Mean     | Error    | StdDev   | Allocated |
|-------------- |---------:|---------:|---------:|----------:|
| TestCsly      | 680.8 μs | 13.60 μs | 21.57 μs |   3.07 KB |
| TestFluent    | 639.9 μs | 12.62 μs | 20.74 μs |   3.08 KB |
| TestGenerated | 641.0 μs | 16.12 μs | 47.54 μs |   3.09 KB |

No real difference here ! maybe test case is not large enough.

with 500 ops : 

| Method        | Mean     | Error    | StdDev   | Allocated |
|-------------- |---------:|---------:|---------:|----------:|
| TestCsly      | 607.3 μs | 14.65 μs | 42.04 μs |   3.07 KB |
| TestFluent    | 629.8 μs | 17.95 μs | 51.78 μs |   3.08 KB |
| TestGenerated | 745.1 μs | 18.32 μs | 54.03 μs |   3.09 KB |

Parsers are stack overflowing above 500 operands
