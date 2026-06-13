---
id: Perso.projects.dendronlike. perf
title: Perso.projects.dendronlike. perf
desc: Slow PWA loading 
updated: 1781333203152
created: 0
---
# PWA Android Splash Screen Cold-Boot Optimization Guide

This document outlines architectural adjustments to eliminate PWA launch hangs (specifically after Android OS reboots/cold starts). The setup optimizes a **Svelte 4 + Rollup** frontend paired with an **ASP.NET Core 8** backend.

---

## 🛠️ Frontend Optimizations (Svelte 4 & Rollup)

The primary goal is breaking down monolithic JavaScript execution blocks to prevent CPU starvation in the Android V8 engine.

### 1. Rollup Configuration (`rollup.config.js`)
Switch output from a single file to a directory to support **ES Module native chunking**.

```javascript
import svelte from 'rollup-plugin-svelte';
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';
import { visualizer } from 'rollup-plugin-visualizer';

export default {
  input: 'src/main.js',
  output: {
    // OLD: file: 'public/build/bundle.js',
    dir: 'public/build',          // Enables multi-file code splitting
    format: 'esm',                 // Required for dynamic code splitting
    sourcemap: true,
    chunkFileNames: '[name]-[hash].js'
  },
  plugins: [
    svelte({ 
      emitCss: false, // do not compile a single css => avoid problems with lazy loading  
			extensions: ['.svelte'],
			compilerOptions: {
				// enable run-time checks when not in production
				dev: !production,	
				enableSourcemap: true,					
			},
			preprocess: sveltePreprocess({
				sourceMap: true,
			  }) }),
    resolve({ browser: true, dedupe: ['svelte'] }),
    commonjs(),
    
    // Generates an interactive chart to audit heavy dependencies
    visualizer({
      filename: 'bundle-analysis.html',
      open: false 
    })
  ]
};
```

### 2. HTML Entry Point (index.html)

Update script orchestration to handle ECMAScript modules and inject an immediate HTML/CSS application shell to force Android to drop the native splash screen instantly.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel='stylesheet' href='/global.css'>
  
  <script type="module" src="/build/main.js"></script>
</head>
<body>
  <div id="app">
    <div class="fallback-splash">
      <div class="spinner"></div>
      <style>
        .fallback-splash {
          display: flex; justify-content: center; align-items: center;
          position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
          background: #121212; z-index: 9999;
        }
        .spinner {
          width: 40px; height: 40px; border: 4px solid #f3f3f3;
          border-top: 4px solid #3498db; border-radius: 50%;
          animation: spin 1s linear infinite;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
      </style>
    </div>
  </div>
</body>
</html>
```
### 3. Dynamic Lazy Loading (Svelte Components)

Wrap internal routes or heavy layout engines into runtime dynamic import() components.

```html
<script>
  let componentPromise;

  function loadHeavyDashboard() {
    // This creates an isolated bundle chunk that only loads when triggered
    componentPromise = import('./Dashboard.svelte').then(res => res.default);
  }
</script>

<button on:click={loadHeavyDashboard}>Go to Dashboard</button>

{#if componentPromise}
  {#await componentPromise}
    <p>Loading application layer...</p>
  {:then DynamicComponent}
    <svelte:component this={DynamicComponent} />
  {:catch error}
    <p>Boot Error: {error.message}</p>
  {/await}
}
```

### 4. Backend Optimizations (ASP.NET Core 8)

Configuring explicit headers forces Chrome/Android WebView to aggressively cache chunks and flags them safely for V8 Binary/Bytecode compilation caching.

Static File Configurations (Program.cs)
Replace standard app.UseStaticFiles() with a pipeline wrapper that protects application state (Service Workers/HTML) while hard-caching static code blocks.

> This alone is not enough to speed up loading as PWA still would have to load, read and compile a big js bundle if not splitted.

```csharp
// Program.cs
using Microsoft.AspNetCore.Builder;
using System;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

// Cache Control String definitions
const string CacheMaxAgeOneYear = "public, max-age=31536000, immutable";
const string DoNotCache = "no-cache, no-store, must-revalidate";

app.UseStaticFiles(new StaticFileOptions
{
    OnPrepareResponse = ctx =>
    {
        var fileName = ctx.File.Name;

        // CRITICAL: Prevent entry points from being permanently frozen in user cache
        if (fileName.Equals("sw.js", StringComparison.OrdinalIgnoreCase) || 
            fileName.Equals("index.html", StringComparison.OrdinalIgnoreCase) ||
            ctx.Context.Request.Path.StartsWithSegments("/manifest.json"))
        {
            ctx.Context.Response.Headers.CacheControl = DoNotCache;
            ctx.Context.Response.Headers.Pragma = "no-cache";
            ctx.Context.Response.Headers.Expires = "0";
        }
        // AGGRESSIVE: Cache split JS chunks, CSS and Media for V8 runtime retention
        else if (ctx.Context.Request.Path.StartsWithSegments("/build") || 
                 fileName.EndsWith(".js") || 
                 fileName.EndsWith(".css"))
        {
            ctx.Context.Response.Headers.CacheControl = CacheMaxAgeOneYear;
        }
    }
});

app.Run();
```

### 5. Validation Checklist

1. Verify Headers: Open Desktop Chrome DevTools -> Network -> click a dynamic chunk (e.g. main-hash.js). Confirm Cache-Control reads public, max-age=31536000, immutable.
2. Verify Chunking: Confirm that the build process generates a set of smaller modules inside public/build/ rather than a monolithic file.
3. Cold Boot Inspection: Connect phone via USB (chrome://inspect/#devices), perform an Android reboot, launch the PWA, and capture a Performance Profile to confirm the initial UI paint fires before the background script processes complete.


### 6. Gemini discussion

https://g.co/gemini/share/1d474717ce1d
