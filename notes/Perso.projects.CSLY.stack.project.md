---
id: Perso.projects.CSLY.stack.project
title: Perso.projects.CSLY.stack.project
desc: feature management
updated: 1782552627556
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

### 🪲`TemplateTests#BasicTemplateTest 2026-06-24`
end of parse detection issue.
[7760837efdeb5882a2d1fac5d8be4ae2258be01d]
(https://github.com/b3b00/csly/commit/7760837efdeb5882a2d1fac5d8be4ae2258be01d)

5 remaining



### 🪲`I18nTests#TestErrorMessage` et 🪲`Issue164#TestErrorMessage`


### 🪲`ErrorMessageAccuracyIssue381Tests#TestAccuracy`

[#381](https://github.com/b3b00/csly/issues/381)

```
      variable = function(someVariable, ""string1"", ""string2"", 
            ""string3"", ""value1"",
            ""string4"", ""value2""
            ""string5"", ""value3"",
            ""string6"", ""value4""
```

should return error stating unexpected "string5", expecting any operator or comma
but
```
unexpected Lparen ('( (line 1, column 27)'). Expecting Id, Lbrack, .
```

parser not choosing furthest along error message ?

tracking found unexpected token errors during parse : 

```
unexpected :: unexpected Id ('variable (line 1, column 8)'). Expecting Lbrack, . -- 10
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting Minus, . -- 21
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting Not, . -- 21
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting String, . -- 21
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting Double, . -- 21
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting Int, . -- 21
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting True, False, . -- 21
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting Empty, . -- 21
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting Lparen, . -- 21
unexpected :: unexpected Id ('function (line 1, column 19)'). Expecting Lbrack, . -- 21
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Equal, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Diff, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Greater, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting GreaterEqual, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Lesser, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting LesserEqual, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Plus, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Or, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Times, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Div, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Set, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Minus, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Id, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting And, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Lbrack, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting Id, Lbrack, . -- 29
unexpected :: unexpected Lparen ('( (line 1, column 27)'). Expecting In, . -- 29
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting Int, . -- 30
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting Minus, . -- 30
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting Not, . -- 30
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting Lbrack, . -- 30
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting Lparen, . -- 30
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting Empty, . -- 30
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting True, False, . -- 30
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting Double, . -- 30
unexpected :: unexpected Id ('someVariable (line 1, column 28)'). Expecting String, . -- 30
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting In, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Lesser, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Lparen, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Set, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Equal, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Greater, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Diff, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting GreaterEqual, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Or, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Minus, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Plus, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Div, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting LesserEqual, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting And, . -- 42
unexpected :: unexpected Comma (', (line 1, column 40)'). Expecting Times, . -- 42
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Empty, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting True, False, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Int, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Double, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Id, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Lbrack, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Not, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Minus, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Id, Lbrack, . -- 44
unexpected :: unexpected String ('"string1" (line 1, column 42)'). Expecting Lparen, . -- 44
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Minus, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Plus, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Div, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Times, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Or, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting And, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting LesserEqual, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Lesser, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting GreaterEqual, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Greater, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Diff, . -- 53
unexpected :: unexpected Comma (', (line 1, column 51)'). Expecting Equal, . -- 53
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Id, Lbrack, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Minus, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Not, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Id, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Double, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Int, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting True, False, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Empty, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Lparen, . -- 55
unexpected :: unexpected String ('"string2" (line 1, column 53)'). Expecting Lbrack, . -- 55
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting And, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Equal, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Diff, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Greater, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting GreaterEqual, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Or, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Lesser, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Minus, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Plus, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Div, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting LesserEqual, . -- 64
unexpected :: unexpected Comma (', (line 1, column 62)'). Expecting Times, . -- 64
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Id, Lbrack, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Lbrack, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Lparen, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Empty, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting True, False, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Int, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Double, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Id, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Not, . -- 80
unexpected :: unexpected String ('"string3" (line 2, column 12)'). Expecting Minus, . -- 80
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Lesser, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Times, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Equal, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Diff, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Greater, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting GreaterEqual, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting LesserEqual, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting And, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Or, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Div, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Plus, . -- 89
unexpected :: unexpected Comma (', (line 2, column 21)'). Expecting Minus, . -- 89
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Id, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Not, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Empty, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Double, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Minus, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Int, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting True, False, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Lbrack, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Id, Lbrack, . -- 91
unexpected :: unexpected String ('"value1" (line 2, column 23)'). Expecting Lparen, . -- 91
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Minus, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Equal, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Diff, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Greater, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting GreaterEqual, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting LesserEqual, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting And, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Or, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Times, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Div, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Lesser, . -- 99
unexpected :: unexpected Comma (', (line 2, column 31)'). Expecting Plus, . -- 99
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Not, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Minus, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Id, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Double, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Int, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting True, False, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Empty, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Lparen, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Lbrack, . -- 114
unexpected :: unexpected String ('"string4" (line 3, column 12)'). Expecting Id, Lbrack, . -- 114
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Times, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Div, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Equal, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Greater, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting GreaterEqual, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Lesser, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting LesserEqual, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting And, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Or, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Minus, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Diff, . -- 123
unexpected :: unexpected Comma (', (line 3, column 21)'). Expecting Plus, . -- 123
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Not, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Minus, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Id, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Double, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Int, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting True, False, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Empty, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Lparen, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Lbrack, . -- 125
unexpected :: unexpected String ('"value2" (line 3, column 23)'). Expecting Id, Lbrack, . -- 125
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Times, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Or, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Diff, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting GreaterEqual, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Lesser, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting LesserEqual, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting And, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Rparen, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Comma, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Minus, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Plus, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Div, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Greater, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Equal, . -- 147

```

if looking at last position (147) errors : 

```
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Times, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Or, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Diff, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting GreaterEqual, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Lesser, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting LesserEqual, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting And, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Rparen, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Comma, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Minus, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Plus, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Div, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Greater, . -- 147
unexpected :: unexpected String ('"string5" (line 4, column 12)'). Expecting Equal, . -- 147
```
so indeed a problem of error accumulation and filtering.

[b7dbb0ea9e60fb9f2912fa58199345cb1738fef1](https://github.com/b3b00/csly/commit/b7dbb0ea9e60fb9f2912fa58199345cb1738fef1)

fixed but parser now returns many errors instead of only one. May be acceptable.

issue #302 is now failing


### 🪲`Issue302Test#Test302`

not founding an issue where needed.



