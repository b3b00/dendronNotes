---
id: 3u3uszvmt5mpwd9oh9vspuo
title: AOT-and-Trimming
desc: AOT and trimming
updated: 1729683213397
created: 1723204851943
---


# benchmarking

Use IndentedWhile as reference language with program 
```
a:=0
while a < 10 do 
	print a
	a := a +1
if a > 145 then
	print a
	skip
	a := a *478
	if a > 125874 then
		a := 1+2+3+4+5+6+7+8+9+10+20+30
		b := true
		print ""bololo""
	else
		b := false
		print ""youpi""
else
	print b
```

## Benchmark dot net

```

BenchmarkDotNet v0.14.0, Windows 10 (10.0.19045.4651/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.107
  [Host]        : .NET 8.0.7 (8.0.724.31311), X64 RyuJIT AVX2
  .NET 8.0      : .NET 8.0.7 (8.0.724.31311), X64 RyuJIT AVX2
  NativeAOT 8.0 : .NET 8.0.7, X64 NativeAOT AVX2


```
| Method     | Job           | Runtime       | Mean     | Error     | StdDev    | Median   | Ratio | RatioSD | Gen0     | Gen1     | Allocated | Alloc Ratio |
|----------- |-------------- |-------------- |---------:|----------:|----------:|---------:|------:|--------:|---------:|---------:|----------:|------------:|
| ParseWhile | .NET 8.0      | .NET 8.0      | 1.306 ms | 0.0291 ms | 0.0810 ms | 1.295 ms |  1.00 |    0.09 | 347.6563 | 148.4375 |   1.59 MB |        1.00 |
| ParseWhile | NativeAOT 8.0 | NativeAOT 8.0 | 1.271 ms | 0.0253 ms | 0.0666 ms | 1.249 ms |  0.98 |    0.08 | 312.5000 | 164.0625 |   1.52 MB |        0.96 |


## custom bench

10_000 loops + build time 

using `dotnet publish -c release`

### no AOT

AOT : 
 - build : 9 ms
 - run : 33688 ms
 - build : 4 ms
 - run : 28960 ms
 - build : 4 ms
 - run : 30292 ms

Legacy :
 - build : 23 ms
 - run : 33151 ms
 - build : 22 ms
 - run : 32984 ms
 - build : 24 ms
 - run : 31154 ms


### AOT

AOT : 
 - build : 3 ms
 - run : 32296 ms
 - build : 3 ms
 - run : 38112 ms
 - build : 2 ms
 - run : 31318 ms
 - build : 1 ms
 - run : 33733 ms
  


legacy : N/A



| Method     | Job           | Runtime       | Mean     | Error     | StdDev    | Median   | Ratio | RatioSD | Gen0     | Gen1    | Allocated | Alloc Ratio |
|----------- |-------------- |-------------- |---------:|----------:|----------:|---------:|------:|--------:|---------:|--------:|----------:|------------:|
| ParseWhile | .NET 8.0      | .NET 8.0      | 1.270 ms | 0.0969 ms | 0.2716 ms | 1.163 ms |  1.04 |    0.28 | 296.8750 | 93.7500 |   1.39 MB |        1.00 |
| ParseWhile | NativeAOT 8.0 | NativeAOT 8.0 | 1.139 ms | 0.0456 ms | 0.1331 ms | 1.086 ms |  0.93 |    0.19 | 332.0313 |  3.9063 |   1.33 MB |        0.96 |

```
BenchmarkDotNet v0.14.0, Windows 10 (10.0.19045.4651/22H2/2022Update)
Intel Core i7-10610U CPU 1.80GHz, 1 CPU, 8 logical and 4 physical cores
.NET SDK 8.0.107
  [Host]        : .NET 8.0.7 (8.0.724.31311), X64 RyuJIT AVX2
  .NET 8.0      : .NET 8.0.7 (8.0.724.31311), X64 RyuJIT AVX2
  NativeAOT 8.0 : .NET 8.0.7, X64 NativeAOT AVX2
```

| Method     | Job           | Runtime       | Mean       | Error    | StdDev    | Median     | Ratio | RatioSD | Gen0     | Gen1    | Allocated | Alloc Ratio |
|----------- |-------------- |-------------- |-----------:|---------:|----------:|-----------:|------:|--------:|---------:|--------:|----------:|------------:|
| ParseWhile | .NET 8.0      | .NET 8.0      | 1,240.8 us | 88.20 us | 245.86 us | 1,160.3 us |  1.03 |    0.27 | 302.7344 | 87.8906 |   1.39 MB |        1.00 |
| ParseWhile | NativeAOT 8.0 | NativeAOT 8.0 |   989.7 us | 25.25 us |  70.80 us |   987.3 us |  0.82 |    0.15 | 326.1719 | 46.8750 |   1.33 MB |        0.96 |
```
// * Warnings *
MultimodalDistribution
  Bencher.ParseWhile: .NET 8.0      -> It seems that the distribution can have several modes (mValue = 3.11)
  Bencher.ParseWhile: NativeAOT 8.0 -> It seems that the distribution is bimodal (mValue = 3.62)

// * Hints *
Outliers
  Bencher.ParseWhile: .NET 8.0      -> 10 outliers were removed (2.14 ms..2.61 ms)
  Bencher.ParseWhile: NativeAOT 8.0 -> 9 outliers were removed (1.23 ms..1.37 ms)
```  