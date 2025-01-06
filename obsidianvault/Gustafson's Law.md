If $s = 1 - p$ is the proportion of execution time that must be sequential, then $S(N)$ is the maximum theoretical speedup achievable by execution on $N$ threads, and is given by
$$
S(N) = N + (1-N) \cdot s
$$
Predicts [[Scalability#Definition Weak Scaling|weak scaling]].

Gustafson's law is optimistic.

![[Pasted image 20241228105436.png]]


## Exercises 2024-10-07

*For this question we use Gustafson's Law to estimate speedup in latency. Suppose we have a program where 4% of the work is not parallelisable. Assuming the rest can be fully parallelised without any overhead, and that the parallel workload is proportional to the amount of processors/threads we use:*

$p = 0.96$
$s = 1 - p = 0.04$

1. *What is the speedup if we run it on a 4-processor machine?*
   $$
   S(4) = 4 + (1 - 4) \cdot 0.04 = 0.04 = 3.88
   $$
2. *What about with 128 processors?*
   $$
   S(128) = 128 + (1 - 128) \cdot 0.04 = 122.92
   $$
3. *What is the smallest number of processors that will give us a speedup of at least 5?*
   6, because:
   
   $S(N) = 5$
   Solving for $N$, we get $N = 5.16$. Since we can't have a fractional number of processors, we round up to 6.
   
4. *What is the smallest number of processors that will give us a speedup of at least 30?*
   32, because:
   
   $S(N) = 30$.
   Solving for $N$, we get $N = 31.21$. Since we can't have a fractional number of processors, we round up to 32.