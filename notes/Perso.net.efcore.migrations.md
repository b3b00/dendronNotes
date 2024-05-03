---
id: Perso.net.efcore.migrations
title: Perso.net.efcore.migrations
desc: EF Core migrations
updated: 1714740490269
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

# Migrations au runtime 

```c#
public static void Main(string[] args)
{
    var host = CreateHostBuilder(args).Build();

    using (var scope = host.Services.CreateScope())
    {
        var db = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
        db.Database.Migrate();
    }

    host.Run();
}
```
[Apply migrations at runtime](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/applying?tabs=dotnet-core-cli#apply-migrations-at-runtime)


# sources

[dotnet et tool](https://learn.microsoft.com/en-us/ef/core/cli/dotnet)
