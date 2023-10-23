---
id: NlcAYfB6Cu
title: Perso-projects-dendronlike-docker-build
desc: Build Docker
updated: 1698065244403
created: 1698065244403
---

Pour construire l'image docker :
```
docker build .
```
```
docker build - < Dockerfile
```
ne fonctionne pas car il lui manque le contexte pour les `COPY`