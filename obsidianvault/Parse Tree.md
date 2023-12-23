In order to design a [[Compiler|compiler]], a way of parsing the text of a program is needed. This task is accomplished by a parser, which uses a [[Backus-Naur Form#Rules|grammar]] to create parse trees.
Parse trees are graphical depictions of [[Backus-Naur Form#Derivation of a grammar|grammatical derivations]].

Let's say we have some grammar for a simple programming language
```
<stmts>        -> <stmt> | <stmt><stmts>
<stmt>         -> <assign>; | <if_stmt>
<assign>       -> id = id
<if_stmt>      -> if(<boolean_expr>)<stmt>
                | if(<boolean_expr>)<stmt>else<stmt>
<boolean_expr> -> id == id
```
*The syntax `id = id` and `id == id` is not proper [[Backus-Naur Form]], but for this purpose it's okay*.

We can do a left-most derivation on the grammar
```
<stmts> => <stmt><stmts>
        => <assign>;<stmts>
        => id = id; <stmts>
        => id = id; <stmt>
        => id = id; <if_stmt>
        => id = id; if(<boolean_expr>)<stmt>else<stmt>
        => id = id; if(id == id) <stmt> else <stmt>
        => id = id; if(id == id) <assign> else <stmt>
        => id = id; if(id == id) id = id; else <stmt>
        => id = id; if(id == id) id = id; else <assign>
        => id = id; if(id == id) id = id; else id = id;
```

and this is valid code
```
threshold = userInput;
if (threshold == criticalValue)
	result = specialValue;
else
	result = userInput;
```

We can do the same derivation as a parse tree
![[Pasted image 20231223123813.png]]

This is problematic though, as the grammar is ambiguous...

### Ambiguous grammar
The grammar above is ambiguous since both of the following code blocks are valid, yet we don't know which if-statement the `else` belongs to.
```
if (value == best)
  if (update == userUpdate)
    userBest = value;
  else
    groupBest = value;
```

```
if (value == best)
  if (update == userUpdate)
    userBest = value;
else
  groupBest = value;
```

The difference in indentation is illustrated by the following parse trees. It should not be possible to construct two different parse trees from the same derivation - herein lies the ambiguity
![[Pasted image 20231223124312.png]]

### Fixing the ambiguous grammar
The problem with the sentence `if(<boolean_expr>)<stmt>else<stmt>` is that we can keep nesting if-statements in the first statement. Let's fix it with the following grammar
```
<stmts>        -> <stmt> | <stmt><stmts>
<stmt>         -> <matched> | <unmatched>
<matched>      -> <assign>;
                | if(<boolean_expr>)<matched>else<matched>
<unmatched>    -> if(<boolean_expr>)<stmt>
                | if(<boolean_expr>)<matched>else<unmatched>
<assign>       -> id = id
<boolean_expr> -> id == id
```

Without a grammar, this sentence seems hard to parse:
`if(value==best) if(update==userUpdate) userBest=value; else groupBest=value;`
However, our new grammar is unambiguous so we can use it. A way to do that, when we already have a sentence that we want to analyze, is to do an upside down parse tree.

![[Pasted image 20231223124848.png]]

By starting with the terminals `id`, `==` and `=`, we can parse the sentence and use bigger and bigger non-terminals. We find that the meaning of the sentence is the first of the two code blocks from above

```
if (value == best)
  if (update == userUpdate)
    userBest = value;
  else
    groupBest = value;
```
