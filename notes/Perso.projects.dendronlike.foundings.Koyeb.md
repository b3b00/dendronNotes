---
id: WQytHWU6JG
title: Perso.projects.dendronlike.foundings.Koyeb
desc: Koyeb deployment quirks
updated: 1707157633028
created: 1696788740487
---


# [Koyeb](https://koyeb.com)

Le passage de Railway à Koyeb est immédiat grâce à la mise en oeuvre de docker.
Seuls les settings ont été modifiés : Koyeb n'accepte pas les ':' dans les noms de variables d'environnement. On remplace les ':' par '_'.

Koyeb publie le port avec la variable  d'environnement `$PORT`.


