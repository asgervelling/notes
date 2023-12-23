## LICS: Assignment 1
*Asger Skov Velling - nbz132*

### Exercises 1.1
> Use $\neg, \rightarrow, \land$ and $\lor$ to express the following declarative sentences in propositional logic; in each case state what your respective propositional atoms $p$, $q$, etc. mean:

##### 1(e)
> Cancer will not be cured unless its cause is determined and a new drug for cancer is found.

$p$: *Cancer will be cured*
$q$: *its cause is determined*
$r$: *a new drug for cancer is found.*

$$
\neg p \rightarrow \neg(q \land r)
$$

##### 1(h)
> Today it will rain or shine, but not both.

$p$: *Today it will rain*
$q$: Today it will shine

$$
(p \lor q) \land \neg(p \land q)
$$
Logically equivalent to $p \oplus q$.

### Exercises 1.2
> Prove the validity of the following sequents:

##### 1(b)
$p \land q \vdash q \land p$

$$
\begin{array}{r@{\quad}l@{\quad}l}
1. & p \land q & \text{Premise} \\
2. & p & \land E_1\ 1 \\ 
3. & q & \land E_2\ 1 \\ 
4. & q \land p & \land I\ 2, 3 \\ 
\hline
& q \land p &  \end{array}
$$

##### 1(f)
$\vdash (p \land q) \rightarrow p$
$$
\begin{array}{r@{\quad}l@{\quad}l}
\hline
1. & (p \land q) & \text{Assumption} \\
2. & p & \land E_1\ 1 \\ 
\hline
3. & (p \land q) \rightarrow p & \rightarrow I\ 2, 3 \\
\hline
& (p \land q) \rightarrow p &  \end{array}
$$

(I don't quite understand why I'm allowed to do the implication introduction on row 3).

##### 1(h)
$p \vdash (p \rightarrow q) \rightarrow q$
$$
\begin{array}{r@{\quad}l@{\quad}l}
1. & p & \text{Premise} \\
\hline
2. & (p \rightarrow q) & \text{Assumption} \\
3. & q & \land E_2\ 2 \\
\hline
4. & (p \rightarrow q) \rightarrow q & \rightarrow I\ 2-3 \\
\hline
& (p \rightarrow q) \rightarrow q & \end{array}
$$

##### 1(l)
$p \rightarrow q, r \rightarrow s \vdash p \lor r \rightarrow q \lor s$


### Note to reader
I am going through some heavy personal issues, to a point where I can't even think straight. I have contacted Studenterservice about the issue, and hope to open a dialogue with them about what to do with my courses. 
If I could get the chance to resubmit this assignment, we can buy some time until I hear their recommendation. I'm sure I'm not the only student that has been in this predicament.