---
id: Perso.net.efcore.using.sqlite
title: Perso.net.efcore.using.sqlite
desc: use Sqlite
updated: 1711621923968
created: 0
---
# use Sqlite database.

## Nuget dependencies

```bash
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### and for migrations 

```bash
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
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
    "WebApiDatabase": "Data Source=/path/to/database.sqlite"
  }
}
```
