There are notations that allow us to write extensions to [[Backus-Naur Form]].
The extensions do not enhance the functionality of BNF, but they do make the grammar definitions more concise.

They are
- `{x}` means zero or more repetitions of `x`. It replaces recursion with a form of iteration.
- `[x]` means `x` is optional (e.g. `x | <empty>`)
- `()` for grouping
- `|` anywhere to mean a choice between alternatives
- Quotes around tokens, if necessary, to distinguish from all these meta-symbols.



### Examples: Rewriting BNF to EBNF
These are the exercises with solutions from [CSCI 305](https://csci305.github.io/lectures/l04_grammars.html).

##### a. The set of all strings consisting of zero or more `a`’s

BNF:
```
<S> ::= a <S> | <empty>
```

EBNF:
```
 <S> ::= {a}
```

##### b. The set of all strings consisting of one or more digits. (Each digit is one of the characters 0 through 9)

BNF:
```
 <S> ::= <digit> | <digit> <S>
 <digit> ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

EBNF:
```
 <S> ::= <digit> {<digit>}
 <digit> ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

##### c. The set of all string consisting of one or more `a`’s, with a comma between each `a` and the next. (There should be no comma before the first or after the last.)
BNF:
```
 <S> ::= a | a , <S>
```
EBNF:
```
 <S> ::= a {, a}
```

##### d. The set of all strings consisting of an open bracket (the symbol `[`) followed by a list of zero or more digits separated by commas, followed by a closing bracket (the symbol `]`).

BNF:
```
 <S> ::= [ <digit-list> ] | [ <empty> ]
 <digit-list> ::= <digit> | <digit> , <digit-list>
 <digit> ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

EBNF:
```
 <S> ::= '[' [<digit> {, <digit>}] ']'
 <digit> ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```
