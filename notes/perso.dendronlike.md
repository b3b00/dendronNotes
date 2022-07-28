---
id: r2yjys60zq4xdw7ereczrhu
title: Online Dendron-like
desc: ''
updated: 1659022437778
created: 1658997866134
traitIds:
  - Perso
---

# Description

Appli web de saisie de notes en ligne qui reprend les idées de [dendron](dendron.so) :
- architecture hiérarchique dénotée par des points dans les noms de fichier
- liaison entre notes 
- backend :
    - **sauvegarde dans un repo git ?**
    - sinon dans une base multi tenant ?
- **export au format dendron**
- **gestion d'une liste de tâches hierarchisées (à l'identique de la structure des notes) par extraction des checkbox.**
  - *ajout d'une syntaxe pour une date d'échéance* (optionel)

autres :
 - accessible offline au moins en lecture 

# Technique

## stack technique

aspnet core + htmx 

## hébérgement 

heroku

## backend

GH : 
 - [OAuth app](https://docs.github.com/en/developers/apps/building-oauth-apps/scopes-for-oauth-apps)
    - pas de problème de stockage. 
        - Comment gérer le renewal
        - Comment conserver côté backend (API) l'access token (session ?)




 - GH PAT  
    - où stocker ce PAT ?



