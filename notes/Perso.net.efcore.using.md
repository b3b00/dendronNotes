---
id: Perso.net.efcore.using
title: Perso.net.efcore.using
desc: using EF Core
updated: 1711623611279
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

see specifics for 
 - in memory [[Perso.net.efcore.using.inMemory]]

## Create a repository

### repository interface
```c#
public interface IParentRepository
{
    public List<ParentEntity> GetParents();

    public ParentEntity? GetParentByName(string name)
}
```

### repository implementation

```C#
public class ParentRepository : IParentRepository
{
    private IConfiguration _configuration;
    public ParentRepository(IConfiguration configuration)
    {
        _configuration = configuration;
     
        //
        // initialize database content
        //
        using (var context = new MyContext(configuration))
        {
            var parents = new List<ParentEntity>
            {
                new ParentEntity
                {
                    Name ="mummy",
                    Children = new List<ChildEntity>()
                    {
                        new ChildEntity() { Name = "son"},
                        new ChildEntity() { Name = "daughter"}
                    }
                }
            };
            context.Parents.AddRange(parents);
            context.SaveChanges();
        }
    }

    public ParentEntity? GetParentByName(string name)
    {
        using (var context = new MyContext(_configuration))
        {
            return context.Parents.FirstOrDefault(x => x.Name == name);
        }
    } 
    public List<ParentEntity> GetParents()
    {
        using (var context = new MyContext(_configuration))
        {
            var list = context.Parents
                // Specifies related entities to include in the query results.
                .Include(a => a.Children)
                .ToList();
            return list;
        }
    }
}
```

## inject db repository to service injection

```c#
services.AddScoped<IParentRepository, ParentRepository>();
```

