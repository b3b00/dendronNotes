---
id: Perso.projects.CSLY.stack.project
title: Perso.projects.CSLY.stack.project
desc: feature management
updated: 1782111400215
created: 0
---

## 22/06/2026

### what's missing

[repeat modifiers](https://github.com/b3b00/csly/wiki/EBNF-parser#repeater-modifiers) : 
- {n} : to repeat n times the same terminal, non terminal , choice or group
- {n-m} : to repeat n (included) to m (included) times the same terminal, non terminal , choice or group

### parser root bug fix
Fixed a bug when `[ParserRoot]` was used to set the root rule (might already be present for `BNF_RECURSIVE_PARSER`) : [6d3b81e3e7d38c633289bd849f45c79779df4ccd](https://github.com/b3b00/csly/commit/6d3b81e3e7d38c633289bd849f45c79779df4ccd)

### trying all unit tests with stack
Moving all unit test and samples from `EBNF_LL_RECURSIVE_DESCENT` to `EBNF_LL_STACK` (ctrl-shift-h).
14 failing unit tests. Seems mainly relates to error management.


