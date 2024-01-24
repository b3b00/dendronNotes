---
id: 74vJ9AEYux
title: Perso.projects.dendronlike.foundings.render
desc: Render deployment
updated: 1706081177866
created: 1697268783132
---

---

---

---

---

---
# [Render](http://Reynders.com) deployments

Le passage de Railway à render est immédiat grâce à la mise en oeuvre de docker.
Seuls les settings ont été modifiés : Render n'accepte pas les ':' dans les noms de variables d'environnement. On remplace les ':' par '_'. houps houps

Render publie le port avec la variable  d'environnement `$PORT`.
