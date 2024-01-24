---
id: WQytHWU6JG
title: Koyeb.com
desc: Koyeb deployment quirks
updated: 1706081148339
created: 1696788740487
---
# [Koyeb](https://koyeb.com)

Le passage de Railway à Koyeb est immédiat grâce à la mise en oeuvre de docker.
Seuls les settings ont été modifiés : Koyeb n'accepte pas les ':' dans les noms de variables d'environnement. On remplace les ':' par '-'.
