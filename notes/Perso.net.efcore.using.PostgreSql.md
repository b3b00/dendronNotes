---
id: Perso.net.efcore.using.PostgreSql
title: Perso.net.efcore.using.PostgreSql
desc: PostgreSql
updated: 1722408466699
created: 0
---
# use PostgreSql database.

## Nuget dependencies

```bash
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

### and for migrations 

```bash
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL.Design
```

See [[Perso.net.efcore.migrations]]


## configure Db Context

In DbContext : 

```c#
    protected override void OnConfiguring
        (DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite(Configuration.GetConnectionString("WebApiDatabase"));
    }
```

using appsettings :

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "ConnectionStrings": {
    "WebApiDatabase": "Host=localhost;Database=database;Username=root;Password=root"
  }
}
```
