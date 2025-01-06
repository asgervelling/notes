If $p$ is the proportion of execution time that benets from parallelisation, then $S(N)$ is
maximum theoretical speedup achievable by execution on $N$ threads (so a fixed workload), and is given by


$$
S(N) = \frac{1}{(1-p) + \frac{p}{N}}
$$

Note that
$$
S(N) \leq \frac{1}{1-p}
$$
Amdahl's law predicts [[Scalability#Definition Strong Scaling|strong scaling]]. That is, how the runtime varies with the number of processors.
Amdahl's law is pessimistic.

![[Pasted image 20241228105410.png]]


## Exercises 2024-10-07

*For this question we use Amdahl's Law to estimate speedup in latency. Suppose we have a program where 4% of the work is not parallelisable. Assuming the rest can be fully parallelised without any overhead:*

1. *What is the speedup if we run it on a 4-processor machine?*
   $N = 4$
   $P = 1 - 0.04 = 0.96$
   $$
   S(N) = \frac{1}{(1-p) + \frac{p}{N}} = \frac{1}{0.04 + \frac{0.96}{4}} \approx 3.57
   $$
2. *What about with 128 processors?*
   $$
   S(128) = \frac{1}{0.04 + \frac{0.96}{128}} \approx 21.05
   $$
3. *What is the smallest number of processors that will give us a speedup of at least 5?*
   6, because:
   $$
   S(N) = 5
   $$
   Solving for $N$:
   $$
   \begin{align}
   S(N) &= 5 \\
   \Rightarrow 5 &= \frac{1}{0.04 + \frac{0.96}{N}} \\
   \Rightarrow 1 &= 5 \cdot (0.04 + \frac{0.96}{N}) \\
   \Rightarrow 1 &= 0.2 + \frac{4.8}{N} \\
   \Rightarrow 0.8 &= \frac{4.8}{N} \\
   \Rightarrow 0.8N &= 4.8 \\
   \Rightarrow N &= \frac{4.8}{0.8} \\
   \Rightarrow N &= 6
   \end{align}
   $$
   
4. *What is the smallest number of processors that will give us a speedup of at least 30?*
   Meaning $S = 30$. A 30 times speedup sounds like a lot. Let's check if it's possible. We know that 
   $$
   S(N) \leq \frac{1}{1-p}
   $$
   Substituting for $N, p$:
   $$
   \begin{align}
   S(30) &\leq \frac{1}{1 - 0.96} \\
   \Rightarrow S(30) &\leq 25
   \end{align}
   $$
   
   Since the limit of $S(N)$ is 25 as $N$ goes towards infinity, we can never get a speedup of at least 30 with this program.

### *Estimating parallel fraction*

*Suppose that a program runs in 50s on one processor (N=1) and in 10s on sixteen processors (N=16) with the same workload. Assuming that the parallel part of the program scales linearly with the number of processors used, what is the parallelisable fraction p?*

Since the workload is fixed, we use Amdahl's law.
The speedup of program 1 over program 2 is 5:

$$
S_{speedup} = \frac{P_1}{P_2} = \frac{50}{10} = 5
$$

$$
\frac{1}{(1-p) + \frac{p}{16}} = 5
$$

Solving for $p$, we get 
$$
p = \frac{64}{75} \approx 0.86
$$