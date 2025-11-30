---
id: Perso.projects.CSLY.generator.note
title: Perso.projects.CSLY.generator.note
desc: Expressions
updated: 1764508232271
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
arg0 = visit n-1(child [0])
if bypass
  return arg0
else
 arg1 =visit n-1(child [1])
arg1 = visit n-1(child [0])

return instance. Visit(arg0, arg1, arg2)


```
