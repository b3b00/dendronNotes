---
id: Perso.projects.liste.debug36
title: Perso.projects.liste.debug36
desc: debug 36
updated: 0
created: 0
---
```
Tree.svelte:49 reactive statement : refreshing tree undefined
Tree.svelte:63 loading tree from BackEnd
Tree.svelte:67 Backend response is  Object
Tree.svelte:70 setting tree and notes in store 
Tree.svelte:75 dendron is now fully loaded Object
Tree.svelte:49 reactive statement : refreshing tree Object
TaskRenderer.svelte:28 RENDERER.onMount() :: item :>sans pub< with id :>b87553caef4018976cf21b93140c632032e7a073<
TaskRenderer.svelte:28 RENDERER.onMount() :: item :>PWA isntallable< with id :>795949c30cb6993dec8f169f661eaeb2d703e376<
TaskRenderer.svelte:28 RENDERER.onMount() :: item :>offline first< with id :>e2849dcc9e3f30d36f56ac3c8757501784c21fbd<
TaskRenderer.svelte:28 RENDERER.onMount() :: item :>svelte + svelte material UI< with id :>c796fd6a1985932240facf27a13c5a28bfa146a4<
TaskRenderer.svelte:25 RENDERER.onMount() :: todo item :>générer un lien avec un fragment : [lz-ts](https://www.npmjs.com/package/lz-ts)< with id:>02321b4b4e46d9594260e65995e3ec83a4da4194< - chekced:>true<
TaskRenderer.svelte:25 RENDERER.onMount() :: todo item :>importer le fragment< with id:>975e1c033ce2947835f82a27d4208cb5e750e4fb< - chekced:>true<
TaskRenderer.svelte:25 RENDERER.onMount() :: todo item :>dans un espace dédié pour ne pas écraser les prefs< with id:>a1d2718d8158f9bcd1c0cb5dcaa76431f3bee2f5< - chekced:>true<
TaskRenderer.svelte:25 RENDERER.onMount() :: todo item :>écran listes importées< with id:>79043f3305d06aefd0dc8e54847d7d15fe62ef99< - chekced:>true<
TaskRenderer.svelte:43 RENDERED.toggle() trying to toggle note :>générer un lien avec un fragment : [lz-ts](https://www.npmjs.com/package/lz-ts)> with id:>02321b4b4e46d9594260e65995e3ec83a4da4194< body:># liste de courses.

<https://github.com/b3b00/liste/>

* sans pub
* PWA isntallable
* offline first
* svelte + svelte material UI

[liste](https://liste-de-courses.pages.dev/)

# partage

* [x] générer un lien avec un fragment : [lz-ts](https://www.npmjs.com/package/lz-ts)
* [x] importer le fragment
* [x] dans un espace dédié pour ne pas écraser les prefs
* [x] écran listes importées

## import / export

Continuer avec l'import/ export pour permettre le backup.
<
TaskToggler.ts:24 visit node  {type: 'listItem', spread: false, checked: null, children: Array(1), position: {…}}
TaskToggler.ts:28 TOGGLER : visit item :>sans pub< with hash :>b87553caef4018976cf21b93140c632032e7a073<
TaskToggler.ts:24 visit node  {type: 'listItem', spread: false, checked: null, children: Array(1), position: {…}}
TaskToggler.ts:28 TOGGLER : visit item :>PWA isntallable< with hash :>795949c30cb6993dec8f169f661eaeb2d703e376<
TaskToggler.ts:24 visit node  {type: 'listItem', spread: false, checked: null, children: Array(1), position: {…}}
TaskToggler.ts:28 TOGGLER : visit item :>offline first< with hash :>e2849dcc9e3f30d36f56ac3c8757501784c21fbd<
TaskToggler.ts:24 visit node  {type: 'listItem', spread: false, checked: null, children: Array(1), position: {…}}
TaskToggler.ts:28 TOGGLER : visit item :>svelte + svelte material UI< with hash :>c796fd6a1985932240facf27a13c5a28bfa146a4<
TaskToggler.ts:24 visit node  {type: 'listItem', spread: false, checked: true, children: Array(1), position: {…}}
TaskToggler.ts:28 TOGGLER : visit item :>générer un lien avec un fragment : < with hash :>3e8d5b6e71c583f1a3e9a871e80c8266c3aa6b4a<
TaskToggler.ts:24 visit node  {type: 'listItem', spread: false, checked: true, children: Array(1), position: {…}}
TaskToggler.ts:28 TOGGLER : visit item :>importer le fragment< with hash :>975e1c033ce2947835f82a27d4208cb5e750e4fb<
TaskToggler.ts:24 visit node  {type: 'listItem', spread: false, checked: true, children: Array(1), position: {…}}
TaskToggler.ts:28 TOGGLER : visit item :>dans un espace dédié pour ne pas écraser les prefs< with hash :>a1d2718d8158f9bcd1c0cb5dcaa76431f3bee2f5<
TaskToggler.ts:24 visit node  {type: 'listItem', spread: false, checked: true, children: Array(1), position: {…}}
TaskToggler.ts:28 TOGGLER : visit item :>écran listes importées< with hash :>79043f3305d06aefd0dc8e54847d7d15fe62ef99<
dendronClient.ts:197 saveNote :: PUT url
dendronClient.ts:211 saveNote :: error :: Failed to fetch TypeError: Failed to fetch
    at dendronClient.ts:198:25
    at tslib.es6.js:147:23
    at Object.next (tslib.es6.js:128:53)
    at tslib.es6.js:121:71
    at new Promise (<anonymous>)
    at ft (tslib.es6.js:117:12)
    at St (dendronClient.ts:193:15)
    at TaskRenderer.svelte:47:31
    at Generator.next (<anonymous>)
    at o (TaskRenderer.svelte:1:18)

```
