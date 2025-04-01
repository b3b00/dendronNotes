---
id: J9GllM45dtH
title: Perso.net.cli.profiling
desc: collect profiling using dot net CLI
updated: 1743499687893
created: 1705136341050
---

## install

``` 
dotnet tool install --global dotnet-trace
``` 

## collect traces

```
dotnet-trace collect -- dotnet run project.csproj
dotnet-trace convert 1brc_yyyymmdd_hhmmss.nettrace --format Speedscope
```

