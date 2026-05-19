---
id: Perso.projects.CSLY.perfs.bench-csly-variants
title: Perso.projects.CSLY.perfs.bench-csly-variants
desc: Bench CSLY variants
updated: 1779175193854
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

| Method     | Mean       | Error    | StdDev    | Gen0     | Gen1     | Allocated |
|----------- |-----------:|---------:|----------:|---------:|---------:|----------:|
| TestCsly   | 2,730.2 μs | 55.51 μs | 148.16 μs | 664.0625 | 660.1563 |   7.99 MB |
| TestFluent |   609.0 μs |  6.47 μs |   5.41 μs | 231.4453 | 159.1797 |   2.78 MB |

As suspected fluent parser is faster and more memory efficient.

__Idea for the future__ : we may be able to build a fluent parser using instrospection leading to best of both world (compactness, ease of writing, type safety and efficiency)


