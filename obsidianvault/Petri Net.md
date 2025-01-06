Popular [[Process Modelling|formal notation]] for describing workflows.

A Petri net is a 4-tuple $(P, T, F, M)$ consisting of:
- A finite set of places $P$ (the circles)
- An initial marking $M \in P \times \mathbb{N}$ 
- A finite set of transitions $T$ (the squares)
- A set of arcs $F$ such that $F \subseteq (P \times T) \cup (T \times P)$

![[Pasted image 20250106093758.png]]
*Figure 1.*

## Semantics

### Input and Output Places
- A place $p$ is called an *input place* of a transition $t$ iff there exists a directed arc from $p$ to $t$.
- Place $p$ is called an *output place* of transition $t$ iff there exists a directed arc from $t$ to $p$.

### Enabled
- A transition $t$ is said to be *enabled* iff each input place $p$ of $t$ contains at least one token.
- An enabled transition may *fire*. If transition $t$ fires, then t consumes one token from each input place $p$ of $t$ and produces one token in each output place $p$ of $t$, yielding a new marking.

## Acceptance Condition
To define when a Petri net is accepting, we usually define a set of *accepting markings* $F \subseteq P \times \mathbb{N}$.

![[Pasted image 20250106094748.png]]
*Figure 2: Finite state automaton where double circles denote accepting states.*

The accepting markings for the model in figure 1, if we were to have the same accepting states as the [[Finite State Automata|automaton]] in figure 2, are as follows:
$$
\{\{ (P2, 1), (P4, 1) \}\}
$$

## Concurrency
