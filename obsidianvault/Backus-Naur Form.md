You can express a grammar in Backus-Naur form.

John Backus: The designer of FORTRAN.
Peter Naur: Danish astronomer turned compute scientist.

### Elements of BNF
- Terminals are simply written out: `while`
- Nonterminals are enclosed in angle brackets: `<statement>`
- Productions are in the form
  - `<nonterminal> ::= <sequence of terminals or nonterminals>
  - `<sentence> ::= <noun phrase><verb phrase>`
- We can use `|` to represent `OR`.


### Example
![[Pasted image 20231102000702.png]]
The fragment above defines a `<floating point>` as an integer, then a full stop, then another integer, i.e. `1.2`.

##### Parse Tree
You can demonstrate how `<floating point>` is structured using a parse tree.
A parse tree is a diagram of the structure of a language language fragment, given a grammar
![[Pasted image 20231102001640.png]]
See how it still goes `<integer>.<integer>`.
In the parse tree above, we are parsing `93.4` as a floating point number.


##### More complex example
It is incomplete. This would be fragment of a grammar for a C-based language.
![[Pasted image 20231102000834.png]]