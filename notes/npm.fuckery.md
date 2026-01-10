---
id: npm.fuckery
title: npm.fuckery
desc: npm fuckery
updated: 1768033973021
created: 0
---
# Write something really smart here.

```bash
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for @parcel/watcher-android-arm64@2.5.1: wanted {"os":"android","arch":"arm64"} (current: {"os":"win32","arch":"x64"})

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! unrs-resolver@1.11.1 postinstall: `napi-postinstall unrs-resolver 1.11.1 check`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the unrs-resolver@1.11.1 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\olduh\AppData\Roaming\npm-cache\_logs\2026-01-10T08_09_15_668Z-debug.log
```

now 

```bash
[webpack-cli] Failed to load 'C:\SAGE\SourcesGit\Projects\trapaie\Front\webpack.config.js' config
[webpack-cli] Error: Cannot find module 'node:fs/promises'
Require stack:
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\readdirp\index.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\chokidar\index.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\fork-ts-checker-webpack-plugin\lib\watch\inclusive-node-watch-file-system.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\fork-ts-checker-webpack-plugin\lib\hooks\tap-after-environment-to-patch-watching.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\fork-ts-checker-webpack-plugin\lib\plugin.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\fork-ts-checker-webpack-plugin\lib\index.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\webpack.config.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\webpack-cli\lib\webpack-cli.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\webpack-cli\lib\bootstrap.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\webpack-cli\bin\cli.js
- C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\webpack\bin\webpack.js
    at Function.Module._resolveFilename (internal/modules/cjs/loader.js:885:15)
    at Function.Module._load (internal/modules/cjs/loader.js:730:27)
    at Module.require (internal/modules/cjs/loader.js:957:19)
    at require (internal/modules/cjs/helpers.js:88:18)
    at Object.<anonymous> (C:\SAGE\SourcesGit\Projects\trapaie\Front\node_modules\readdirp\index.js:6:20)
    at Module._compile (internal/modules/cjs/loader.js:1068:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1097:10)
    at Module.load (internal/modules/cjs/loader.js:933:32)
    at Function.Module._load (internal/modules/cjs/loader.js:774:14)
    at Module.require (internal/modules/cjs/loader.js:957:19) {
  code: 'MODULE_NOT_FOUND',
  requireStack: [
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\readdirp\\index.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\chokidar\\index.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\fork-ts-checker-webpack-plugin\\lib\\watch\\inclusive-node-watch-file-system.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\fork-ts-checker-webpack-plugin\\lib\\hooks\\tap-after-environment-to-patch-watching.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\fork-ts-checker-webpack-plugin\\lib\\plugin.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\fork-ts-checker-webpack-plugin\\lib\\index.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\webpack.config.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\webpack-cli\\lib\\webpack-cli.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\webpack-cli\\lib\\bootstrap.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\webpack-cli\\bin\\cli.js',
    'C:\\SAGE\\SourcesGit\\Projects\\trapaie\\Front\\node_modules\\webpack\\bin\\webpack.js'
  ]
}
npm ERR! code ELIFECYCLE
npm ERR! errno 2
npm ERR! sop-react@0.0.1 build: `SET NODE_ENV=development&&.\node_modules\.bin\webpack --watch`
npm ERR! Exit status 2
npm ERR!
npm ERR! Failed at the sop-react@0.0.1 build script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

```
