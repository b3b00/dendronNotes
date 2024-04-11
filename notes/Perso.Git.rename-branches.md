---
id: Perso.Git.rename-branches
title: Perso.Git.rename-branches
desc: git rename branches
updated: 0
created: 0
---
# How to rename branches.

## local 

```bash
git branch -m old new
```

## remote

No way to directly rename a remote branch. you need to delete and recreate
1. s'assurer d'être à jour de la branche en local
2. renommer la branche en local 
```bash
git branch -l old new
``` 
3. supprimer la branche en remote 
```bash
git push origin --delete old
```
4. pousser la branche avec son nouveau nom
```bash
git push -u origin new
```
[source](https://phoenixnap.com/kb/how-to-rename-git-branch-local-remote)

