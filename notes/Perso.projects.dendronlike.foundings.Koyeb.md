---
id: WQytHWU6JG
title: Perso.projects.dendronlike.foundings.Koyeb
desc: Koyeb deployment quirks
updated: 1738697617648
created: 1696788740487
---


# [Koyeb](https://koyeb.com)

## settings

Le passage de Railway à Koyeb est immédiat grâce à la mise en oeuvre de docker.
Seuls les settings ont été modifiés : Koyeb n'accepte pas les ':' dans les noms de variables d'environnement. On remplace les ':' par '_'.

```toml
github_tokenUrl=https://github.com/login/oauth/access_token
github_authorizeUrl=https://github.com/login/oauth/authorize
github_clientId=xxxxxxxxxxxxxxxxxxxx
github_clientSecret=xxxxxxxxxxxxxxxxxxxxxxxxx
github_startUrl=https://localhost:5003/Index
github_redirectUrl=https://localhost:5003/auth
github_LogoutUrl=https://api.github.com/applications/
ASPNETCORE_HTTPS_PORT=5003
PORT=5003
``` 

Koyeb publie le port avec la variable  d'environnement `$PORT`.

## build
```bash
docker buildx build -t dendron2 -f Dockerfile .
``` 
## run
```bash
sudo docker run  --env-file env.env -p 5003:5003 dendron
```
