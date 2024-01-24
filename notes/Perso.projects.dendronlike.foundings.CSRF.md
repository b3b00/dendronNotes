---
id: 12OESVeNgR
title: HTMX & CSRF
desc: Razor/Htmx CSRF configuration
updated: 1698738003415
created: 1696788604478
---
# CSRF

Les POST aboutissent à des ``400`` si on ne configure pas correctement l'antiforgery.



[anti forgery tokens](https://khalidabuhakmeh.com/htmx-requests-with-aspnet-core-anti-forgery-tokens)

le tag helper ne semble pas correctement fonctionner. Il faut mieux ajouter le meta à la main

```html
<meta name="htmx-config" content='{"indicatorClass":"htmx-indicator","historyCacheSize":20,"antiForgery":{"formFieldName":"__RequestVerificationToken","headerName":"RequestVerificationToken","requestToken":"<token>"}}' />
``` 


## ajout d'un middleware pour ajouter le header anti forgery :

```c#
app.Use(async (context, next) =>
{
    var initialBody = context.Request.Body;

    using (var bodyReader = new StreamReader(context.Request.Body))
    {
        var forgeryService = context.RequestServices.GetService<IAntiforgery>();
        var forge = forgeryService?.GetAndStoreTokens(context);
        context.Response.Headers[forge.HeaderName] = forge.RequestToken;
        await next.Invoke();
        context.Request.Body = initialBody;
        
    }
});
```


## en dev : désactivation CSRF token


```csharp
builder.Services.AddRazorPages(o => {
    // this is to make demos easier
    // don't do this in production
    o.Conventions.ConfigureFilter(new IgnoreAntiforgeryTokenAttribute());
});

```
