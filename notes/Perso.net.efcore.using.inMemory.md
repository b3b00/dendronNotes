---
id: Perso.net.efcore.using.inMemory
title: Perso.net.efcore.using.inMemory
desc: using in memory EF database
updated: 1711616884810
created: 0
---
# use in memory database.

## Nuget dependencies

```bash
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### and for migrations 

no need for migrations

## configure Db Context

In DbContext : 

```c#
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseInMemoryDatabase(databaseName: "MyDb");
    }
```


