---
id: Perso.net.efcore.using
title: Perso.net.efcore.using
desc: using EF Core
updated: 0
created: 0
---
# How to configure EF Core.

## Create some entity clases

```c# 
public class ParentEntity
{
    public int Id { get; set; }
    
    public string Name { get; set; }
    
    public IList<ChildEntity> Children { get; set; }
}

public class ChildEntity
{
    public int Id { get; set; }
    
    public string Name { get; set; }
    
    public ParentEntity Parent { get; set; }
}
```

## Create a Db Context

```c#
public class MyContext: DbContext
{
    
    protected readonly IConfiguration Configuration;

    public MyContext(IConfiguration configuration)
    {
        Configuration = configuration;
    }
    protected override void OnConfiguring
        (DbContextOptionsBuilder optionsBuilder)
    {
        // here configure database specifics using optionsBuilder 
    }
    public DbSet<ParentEntity> Parents { get; set; }
    public DbSet<ChildEntity> Children { get; set; }
}
```

## Create a repository
