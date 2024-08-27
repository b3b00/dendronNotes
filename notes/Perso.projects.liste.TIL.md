---
id: Perso.projects.liste.TIL
title: Perso.projects.liste.TIL
desc: TIL
updated: 1724752481202
created: 0
---
# Service worker life cycle.

[service worker lifecycle](https://web.dev/articles/service-worker-lifecycle?hl=fr)

La mise à jour d'un service worker (et donc de la PWA) se fait en 2 phases. La nouvelle version est en état "waiting" jusqu'au prochain lancement. La maj n'est donc pas immédiatement visible.

On peut supprimer l'étape "waiting" en le demandant explicitement lors de l'installation du worker : [Skip the waiting phase](https://web.dev/articles/service-worker-lifecycle?hl=fr#skip_the_waiting_phase)

```js
self.addEventListener('install', function(event) {
  // skipwaiting to avoid the double reload before update get up 
  self.skipWaiting();
  // Perform install steps
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(function(cache) {
        console.log('Opened cache', cache, urlsToCache);
        return cache.addAll(urlsToCache).then(function() {
          console.log('urls have been added to cache');
        }).catch(function(reason) {
          console.log(`beuahah cache failed ; ${reason}`)
        });
      })
  );
});
```

 

# google chrome auto translate

On peut désactiver la traduction automatique des pages par Google Chrome en ajoutant des meta dans le head de la page : 

```html
	<meta name="google" value="notranslate" />
	<meta http-equiv="Content-Language" content="fr_FR" />
```
