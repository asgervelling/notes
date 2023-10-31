Amdahl's law gives the maximum theoretical speedup in terms of latency for a system that can be sped up with parallelization.

given
$N$: Number of threads
$p$: The proportion of execution time that benefits from parallelization

$$
S(N) = \frac{1}{(1-p) + \frac{p}{N}}
$$
where
$$
S(N) \leq \frac{1}{1-p}
$$
