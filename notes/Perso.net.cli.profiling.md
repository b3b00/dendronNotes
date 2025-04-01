---
id: J9GllM45dtH
title: Perso.net.cli.profiling
desc: collect profiling using dot net CLI
updated: 1743506236990
created: 1705136341050
---

## install

``` 
dotnet tool install --global dotnet-trace
``` 

## collect traces

```
dotnet-trace collect -- dotnet run project.csproj
dotnet-trace convert dotnet_<yyyymmdd>_<hhmmss>.nettrace --format Speedscope
```
``` 
dotnet build -c release project.csproj
dotnet-trace collect -- bin/release/net7.0/project
dotnet-trace convert project_<yyyymmdd>_<hhmmss>.nettrace --format Speedscope``  
```
## view 

**online**
[speedscope](https://www.speedscope.app/)

**install locally**
``` 
npm i -g speedscope
speescope
``` 
