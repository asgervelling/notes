You can express a grammar in Backus-Naur form.

John Backus: The designer of FORTRAN.
Peter Naur: Danish astronomer turned compute scientist.

### Rules
Here is a rule in a grammar
`<digit> -> 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9`

- In this case, the right-hand side consists of non-terminals, meaning they cannot be expanded further.
  - In the context of grammars, they are also called lexemes.
- The left-hand side always consists of non-terminals.
  - They have brackets around them to indicate that they can be expanded into something else.

The rule is saying that a digit can be expanded into $0 \lor 1\ \lor\ ... \lor\ 9$.

##### Recursive rules
Let's define a rule for natural numbers
`<nat> -> <digit> | <digit><nat>`.

It states that a natural number expands to either a digit (base case) or a digit followed by a natural number (recursive case).

##### Derivation of a grammar
We can do a left-most derivation on it. We arbitrarily choose cases of the expansion, such as
```
<nat> => <digit><nat>
      => 9<nat>
      => 9<digit><nat>
      => 93<nat>
      => 93<digit>
      => 930
```

Our grammar has a problem. It is possible for us to expand it into a number starting with 0, which is probably not what we want
```
<nat> => <digit><nat>
      => 0<nat>
      => 0<digit>
      => 02
```

Therefore, let's define a better grammar consisting of four rules
```
<nat> -> <digit> | <nonzero><digits>
<digits> -> <digit> | <digit><digits>
<digit> -> 0 | <nonzero>
<nonzero> -> 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

This grammar is saying that a number can only start with a 0 if it is not followed by something else. We can still use 0, but not as the first token.


### Operand Precedence and Associativity
Here is an [exercise](https://csci305.github.io/lectures/l07_syntax.html) in precedence and associativity.

Given the following grammar:

```
<exp>     ::= <exp> + <mulexp> | <mulexp>
<mulexp>  ::= <mulexp> * <rootexp> | <rootexp>
<rootexp> ::= ( <exp> ) | a | b | c
```

Modify it as follows:
- Add a left-associative operator % between + and * in precedence.
- Add a right-associative operator = at lower precedence than any of the other operators

Take a look at the solution and see how it works
```
<exp> ::= <addexp> = <exp> | <addexp>
<addexp> ::= <addexp> + <modexp> | <modexp>
<modexp> ::= <modexp> % <mulexp> | <mulexp>
<mulexp>  ::= <mulexp> * <rootexp> | <rootexp>
<rootexp> ::= ( <exp> ) | a | b | c
```

