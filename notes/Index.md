---
id: esVc3J9BOU
title: Perso-projects-dendronlike-docker-run
desc: run docker
updated: 1698079887407
created: 1698079887407
---
```bash
docker run --enf-file env.env -p 5003:5003 dendron
``

```
github_tokenUrl=https://github.com/login/oauth/access_token
github_authorizeUrl=https://github.com/login/oauth/authorize
github_clientId=
github_clientSecret=
github_startUrl=https://localhost:5003/Index
github_redirectUrl=https://localhost:5003/auth
github_LogoutUrl=https://api.github.com/applications/
PORT=5003
```

ne fonctionne pas à cause de https. PORT donne le port http pas https.

