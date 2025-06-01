---
id: Perso.projects.CSLY.stack.benchEBNF
title: Perso.projects.CSLY.stack.benchEBNF
desc: benchmakrs EBNF
updated: 0
created: 0
---
# EBNF : after profiling and using a managed stack

| Method    | Mean     | Error    | StdDev   | Ratio        | RatioSD | Gen0       | Gen1      | Gen2     | Allocated | Alloc Ratio |
|---------- |---------:|---------:|---------:|-------------:|--------:|-----------:|----------:|---------:|----------:|------------:|
| recursive | 37.09 ms | 1.486 ms | 4.358 ms |     baseline |         | 18500.0000 |  500.0000 | 166.6667 |  79.61 MB |             |
| stacked   | 29.33 ms | 0.453 ms | 0.402 ms | 1.45x faster |   0.14x | 18666.6667 | 1000.0000 | 333.3333 |  79.88 MB |  1.00x more |

**To be double checked**
