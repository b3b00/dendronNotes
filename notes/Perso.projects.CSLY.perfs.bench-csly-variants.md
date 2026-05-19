---
id: Perso.projects.CSLY.perfs.bench-csly-variants
title: Perso.projects.CSLY.perfs.bench-csly-variants
desc: Bench CSLY variants
updated: 0
created: 0
---
#Goal

CSLY may be used in 3 different ways :
 1. the classical way using reflection
 2. the fluent way using the fluent API to build the parser
 3. the generative way, using csly.generator to build a static parser at build time

Goal is to bench all of them to find if any significative performance difference

## expected outcome

**fluent vs classical** : fluent built parser do not use instrospection to call visitor (instead they use direct lambda call with no type check), we can inspect that fluent parser is faster, and maybe more memory efficient ?.

generative vs fluent/classical : ?

## 
