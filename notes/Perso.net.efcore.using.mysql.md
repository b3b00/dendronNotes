---
id: Perso.net.efcore.using.mysql
title: Perso.net.efcore.using.mysql
desc: MySQL
updated: 1711634568868
created: 0
---
# use MySql database.

## Nuget dependencies

```bash
dotnet add package MySql.EntityFrameworkCore
```

### and for migrations 

No particular need.

See [[Perso.net.efcore.migrations]]


## configure Db Context

In DbContext : 

```c#
    protected override void OnConfiguring
        (DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseMySQL(Configuration.GetConnectionString("WebApiDatabase"));
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
    "WebApiDatabase": "server=localhost;database=library;user=user;password=password"
  }
}
```
