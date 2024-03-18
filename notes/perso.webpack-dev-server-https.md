---
id: perso.webpack-dev-server-https
title: perso.webpack-dev-server-https
desc: new note
updated: 1710771268302
created: 0
---
[how to get webpack dev server https work](https://stackoverflow.com/questions/26663404/webpack-dev-server-running-on-https-web-sockets-secure)

### générer le certificat

```bash
npx mkcert create-ca
npx mkcert create-cert
```

edit webpack.config.js
Dans 

## configurer le dev server webpack

```javascript
const fs = require('fs')
```

```javascript
devServer: {
    // ...
    https: {
        key: fs.readFileSync("cert.key"),
        cert: fs.readFileSync("cert.crt"),
        ca: fs.readFileSync("ca.crt"),
    },
    // ....
},
```

## installer le certificat

1. double clic sur `ca.cert`
2. isntaller le certificat...
3. utilisateur
4. choisir le magasin
5. choisir "Autorités de certification racines de confiance"

