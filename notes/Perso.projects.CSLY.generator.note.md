---
id: Perso.projects.CSLY.generator.note
title: Perso.projects.CSLY.generator.note
desc: Expressions
updated: 1764508019674
created: 0
---
# expressions parsing

# parser

## infix
````
r0 = parse n-1

r1 = parse operator

If not r1
  return new bypass node (r0) 
else

r3 =......
````

# visitor

## infix

```
if bypass
  visit n-1(child [0])
else
 visit child 0..2
return instance. Visit(arg0....) 


```
