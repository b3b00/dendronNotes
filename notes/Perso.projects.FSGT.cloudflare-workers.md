---
id: y6nyisoi5tajk6mb5knke3k
title: Perso.projects.FSGT.cloudflare-workers
desc: FSGT Calendars on cloudFlare workers ***
updated: 1681393569623
created: 1663922310241
---




[workers sites](https://developers.cloudflare.com/workers/platform/sites/start-from-worker)

# KV & assets static

## static assets folder

create folder `./public` that will contain static assets

## wrangler.toml configuration

Add :

```toml
[site]
bucket="./public"
```
This will makes wrangler push assets from ./public to the bound KV.

when pushing assets wrangler generates keys containing unique id between name and extension.


## worker assets handler

```javascript

const GetAssetHandler = async function(request) {
    const assetName = request.params.name
    return await GetAsset(assetName)
}



/**
 * here is the real magic for assets
 * */
const GetAsset = async function(assetName) {
    // get the list of assets
    const assets = await DEVASSETS.list()    
    // split asset name (name, extension)  : brittle if you consider assets with many dots.   
    const items = assetName.split('.')
    const name = items[0]
    const extension = items[1]

    // looks for assets with leading name and trailing extension 
    // this is because when pushing assets wrangler generates keys containing unique ids between name and extension    
    let matches = assets.keys.filter(
        x => x.name.startsWith(name) && x.name.endsWith(extension)
    )
    

    if (matches && matches.length > 0) {
        // get the first matching asset
        assetName = matches[0].name;
        
        const assetBody = await DEVASSETS.get(assetName)
        if (assetBody) {
          
          
          // set content type header 
            let response = new Response(assetBody)
            if (assetName.endsWith('.html')) {        
                response.headers.set('Content-type', 'text/html')
            } else if (assetName.endsWith('.css')) {
                response.headers.set('Content-type', 'text/css')                
            } else if (assetName.endsWith('.js')) {
                response.headers.set('Content-type', 'text/javascript')
            } else {
                response.headers.set('content-type', 'text/plain')
            }
            return response;
        } else {
            // asset is empty
            new Response(`404, ${assetName} not found!`, { status: 404 })
        }
    } else {
        // no asset found
        new Response(`404, ${assetName} not found!`, { status: 404 })
    }
}

const index = async function(request) {
    // returns static asset index.html
    return await GetAsset('index.html')
}

router.get('/', index)

router.get('/assets/:name', GetAssetHandler)
```



# better dev experience

use `npx wrangler@beta dev ` 

# D1 databases

[D1 getting started](https://developers.cloudflare.com/d1/get-started/)




[[d1_databases]]
binding = "<BINDING_NAME>"
database_name = "<DATABASE_NAME>"
database_id = "<UUID>"

⚠️ utiliser les worker ES6 Module
pour avoir accès à l'environnement et donc la base D1.

```javascript

import { Router } from 'itty-router'

const router = Router()

const table = async function(request, env, context) {
  const { results } = await env.MYBASE.prepare(
    "SELECT * FROM myTable"
  )    
    .all();
  return Response.json(results);
}

router.get('/table',table);


export default {
  async fetch(request, environment, context) {
    return router.handle(request, environment, context);
  },
  async scheduled(controller, environment, context) {
    // await doATask();
  }
}

```

et donc pour également accéder au KV : 

```javascript
const GetAssetHandler = async function(request, env, context) {
    const assetName = request.params.name
    return await GetAsset(assetName, env)
}

const GetAsset = async function(assetName, env) {  
    const assets = await env.DEVASSETS.list()    
    console.log(env.DEVASSETS);
    ....
}
```
