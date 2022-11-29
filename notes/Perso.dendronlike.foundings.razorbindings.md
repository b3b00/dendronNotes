---
id: eurzrk1bhilnotouzsfg36q
title: dendronlike-foundings-razorbindings
desc: 'Razor Bindings'
updated: 1659527371015
created: 1659527230794
traitIds:
  - Perso
---
 
 pour pouvoir utiliser correctement le ```asp-for```
 il faut avoir déclarer un binding sur l'attribut du modele

```xml
<input type="text"
       asp-for="Query"
       id="query"
       autocomplete="off"
       hx-get="@Url.Page("ChooseRepo")"
       hx-target="#repositories"
       hx-trigger="keyup changed delay:250ms"
       placeholder="Search"
       class="form-control"
       aria-label="Username"
       aria-describedby="search-addon"/>
```

 ```csharp
     [BindProperty(SupportsGet = true)]
    public string Query { get; set; }
```