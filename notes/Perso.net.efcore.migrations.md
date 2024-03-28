---
id: Perso.net.efcore.migrations
title: Perso.net.efcore.migrations
desc: EF Core migrations
updated: 1711614132607
created: 0
---
# Ef dotnet tool

la génération des migrations de base EF utilise un outil dotnet 

## instalation 

### installation
```
dotnet tool install --global dotnet-ef
```

### mise à jour 

```
dotnet tool update --global dotnet-ef
```

### ajout des références

```powershell
dotnet add package Microsoft.EntityFrameworkCore.Design
# et pour sqlite par exemple
dotnet add package Microsoft.EntityFrameworkCore.Sqlite.Design
```

## génération d'une migration

```
dotnet ef migrations add migrationName
```

## application d'une migration

```powershell
dotnet ef database update
```

# sources

[dotnet et tool](https://learn.microsoft.com/en-us/ef/core/cli/dotnet)
