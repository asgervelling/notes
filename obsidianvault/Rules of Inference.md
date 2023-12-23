In [[Propositional Logic]], rules of inference take you from a set of premises to a conclusion.

$$
\begin{array}{c}
\text{Premise 1} \\
\text{Premise 2} \\
\vdots \\
\text{Premise n} \\
\hline
\therefore \text{Conclusion}
\end{array}
$$

We are able to deduce a conclusion from the premises.
The three dots $\therefore$ are pronounced *therefore*.
Here's another one:

$$
\begin{array}{c}
\text{If it rains, I will get wet} \\
\text{It's raining} \\
\hline
\therefore \text{I will get wet}
\end{array}
$$

Or with logical symbols:

$$
\begin{array}{c}
p \rightarrow q \\
p \\
\hline
\therefore q
\end{array}
$$

### Formally
- A set of *premises* $p_1,\ p_2,\ p_n$,  prove some conclusion $q$ in an argument.
  $$
  (p_1 \land p_2 \land\ ...\ \land p_n) \rightarrow q
  $$
- An argument is *valid* if the premises logically entail the conclusion.


### The rules of inference
They can be used to prove the validity of your arguments. 
When first learning about them, just accept them. You can try to understand why they work later.

##### Modus Ponens (MPP)
*AKA. the antecedent*.

$$
\begin{array}{c}
p \rightarrow q\\
p \\
\hline
\therefore q
\end{array}
$$
That's like the example of "If it is raining...".

##### Modus Tollens (MTT)
The opposite of modus ponens. 

$$
\begin{array}{c}
p \rightarrow q \\
\neg q \\
\hline
\therefore \neg p
\end{array}
$$
$p \rightarrow q$ is logically equivalent to $\neg p \rightarrow \neg q$, so modus tollens is like doing modus ponens on the [[Contrapositive|contrapositive]].


##### Hypothetical Syllogism (HS)
Just like with the [[Law of Transitivity]],

$$
\begin{array}{c}
p \rightarrow q \\
q \rightarrow r \\
\hline
\therefore p \rightarrow r
\end{array}
$$
This one is not used too often. If you didn't use it, you could still prove all provable arguments, using multiple modus ponens.

##### Disjunctive Syllogism (DS)
$$
\begin{array}{c}
p \lor q \\
\neg p \\
\hline
\therefore q
\end{array}
$$

##### Addition
$$
\begin{array}{c}
p \\
\hline
\therefore p \lor q
\end{array}
$$

##### Simplification
$$
\begin{array}{c}
p \rightarrow q \\
p \\
\hline
\therefore q
\end{array}
$$

##### Implication Introduction
The Implication Introduction rule, also known as the Deduction Theorem, is a fundamental principle in logic that allows you to introduce an implication in a proof. The general form of this rule is as follows:

$$
\frac{{\text{{Assumption: }} A}}{{A \rightarrow B}}
$$

The rule states that if you assume proposition \(A\), then you can conclude an implication: If \(A\) is true, then \(B\) must also be true.

**Example:**

Suppose you have assumed $p$ in a proof. Using the Implication Introduction rule, you can then conclude that $(p \land q) \rightarrow p$.

1. **Assumption:** $p$
2. **Implication Introduction:** $(p \land q) \rightarrow p$

This rule is a key step in constructing conditional statements within logical proofs.
