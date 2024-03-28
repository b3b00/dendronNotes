---
id: Perso.net.efcore.using.inMemory
title: Perso.net.efcore.using.inMemory
desc: using in memory EF database
updated: 0
created: 0
---
# use in memory database.

## Nuget dependencies

```bash
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### and for migrations 

see [[Perso.net.efcore.migrations]]

```bash
dotnet add package Microsoft.EntityFrameworkCore.Sqlite.Design
```

## configure Db Context

In DbContext : 

```c#
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseInMemoryDatabase(databaseName: "AuthorDb");
    }
```


