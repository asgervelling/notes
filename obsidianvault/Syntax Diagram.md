A syntax diagram is a simple diagram with directions and branches, depicting a grammar.
It is a chain of boxes for non-terminals and ovals for terminals.

![[Pasted image 20231224154929.png]]

### Concepts
1. **Sequence**
   The `<if_stmt>` example above is a sequence, the simplest of the varieties.

2. Bypass
   We can skip or bypass parts of the chain
   ![[Pasted image 20231224155130.png]]

3. We may branch out
   ![[Pasted image 20231224155157.png]]

4. We can loop, representing recursion or iteration
   ![[Pasted image 20231224155221.png]]

### Examples
These are the exercises with solutions from [CSCI 305](https://csci305.github.io/lectures/l04_grammars.html).
Both BNF and EBNF grammars have been included for good measure.

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
