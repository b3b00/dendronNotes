---
id: Perso.projects.CSLY.stack.project
title: Perso.projects.CSLY.stack.project
desc: feature management
updated: 1782229678561
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

#### `EBNFStackTests#TestParseBuild`
Bad assert : __fixed__

### 🪲`Issue259Tests#Issue259StackTest` 
Bad error reporting when infix expression parse fails at right operand level : __fixed__

[df49ddeceaf6b081cba20c2ca6c212ae85e2a7ca](https://github.com/b3b00/csly/commit/df49ddeceaf6b081cba20c2ca6c212ae85e2a7ca)



### 🪲`Issue302Test.Test302Stack` 
Bad error management when parse is ok but not ended : __fixed__
[9a82432767f9034b32a43eaac6c902f2280c4495](https://github.com/b3b00/csly/commit/9a82432767f9034b32a43eaac6c902f2280c4495)
Remaining : 11 failures, mainly expression parsing related.

### 🪲 `ExplicitTokensTests#BuildExpressionParserTest`

Left associativity was badly managed. Use `ExpressionRuleManager<IN, OUT>.ManageExpressionRules` instead.

[5debba177ae9ae6cc0e9986d1fc8e8a88a4d7554](https://github.com/b3b00/csly/commit/5debba177ae9ae6cc0e9986d1fc8e8a88a4d7554)

9 failures remaining.

### 🪲`ExplicitTokensTests#TestExplicitGroupsUnexpectedToken` 

```
root : ('a' 'b')* discard
discard : ('c' 'd'[d])?
```
parsing
```
a b a b c d a
```

expecting error
```
unexpected c ('c (line 0, column 8)'). Expecting 'a', .
```
 - `EBNF_LL_RECURSIVE_DESCENT` : OK
 - `EBNF_LL_STACK` : KO => `unexpected a (line 0, column 12) Nop.` 

Strangely `EBNF_LL_STACK` seems more logic.

Global strategy for error should be to display the further error. Which is to say STACK parser is better at this then RECURSIVE.
When parsing with stack parser we encounter these unextectedTokenErrors : 
 - `unexpected token: Lex :: 0 [c] @line 0, column 8 on channel 0`
 - `unexpected token: Lex :: 0 [a] @line 0, column 12 on channel 0` 

Stack parser seems to chose the first error and recursive


### 🪲`IssuesTests#Issue277Test` 

bad repetition typing : values were taking precedence over groups

[a90c2fd90fbb0e76ffadcbce7eb1b2d339daacfd](https://github.com/b3b00/csly/commit/a90c2fd90fbb0e76ffadcbce7eb1b2d339daacfd)

### 🪲`ExpressionGeneratorTests#TestPrefix` 

Rule for nonterminal `XXXXX_expressions` had no NonTerminalName.

[b3701731bf6f37539376c710665c724b64a59b8a](https://github.com/b3b00/csly/commit/b3701731bf6f37539376c710665c724b64a59b8a)


7 remaining


### 🪲`IndentedWhileTests#TestMissingUIndents` 

indents were not auto close when `[AutoCloseIndentations]` for `EBNFStackDescentSyntaxParser`

[2b76a2e0b4732f140352810256d989387ca8b80f](https://github.com/b3b00/csly/commit/2b76a2e0b4732f140352810256d989387ca8b80f)


6 remaining


