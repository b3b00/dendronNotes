---
id: lc11vatl9v4rm8kocfm0ke6
title: Perso.projects.dendronlike.foundings.oauth
desc: middleware OAuth2 GitHub
updated: 1711636027002
created: 1659685379252
---

il ne semble pas exister de middleware oauth permettant d'accéder simplement à l'access_token retourner par le server oauth.

mise en place d'un middleware minimal : 

https://github.com/b3b00/dendrOnline/blob/main/GitHubOAuthMiddleWare/GitHubOAuthMiddleware.cs

Il doit pouvoir fonctionner avec de nombreux provider oauth avec un travail de configuration.
