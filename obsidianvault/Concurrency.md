When multiple things happen at once.
From a programming perspective, concurrency means having multiple [[Logical Control Flow]]s executing simultaneously.
That's what the word means, anyway. In computing, there is a difference between concurrency and [[Parallelism]].
A concurrent program will multitask really fast, like a human: Do a bit of work on [[Thread]] A, then thread B, then thread A, etc. In computing, this is called interleaving.
[[Process]]es and [[Thread]]s are examples of concurrent execution.

##### Why we use concurrency
We want to make our programs faster, either measured by [[Latency]] or [[Throughput]].
We use the ideas of [[Scalability]]. An exam question might be "Is this function scalable?""
Things have been said about it:
[[Amdahl's Law]]
[[Gustafson's Law]]

##### Classic problems of concurrency
- **Races**: Outcome depends on arbitrary scheduling decisions elsewhere.
	- **Example**: Outcome depends on arbitrary scheduling decisions elsewhere.
- **Deadlocks**: Resource allocation prevents forward progress.
	- **Example**: Traffic gridlock (and programs generally cannot reverse!).
- **Starvation**: External events or scheduling prevents forward progress.
	- **Example**: Someone always jumping ahead in line.
	- Also known as *livelock* or *fairness*.

Each CPU core executes a single logical control flow.

##### Inter-Thread Communication
We can send data from one thread to another using communication channel and signals.

Other things to read up on:
[[Lock]]
[[Lock]]

##### Threaded Fibonacci function
```c
int fib(int n) {
  if (n < 2) {
    return 1;
  } else {
    return fib(n-1) + fib(n-2)
  }
}
```
Goal: Calculate multiple calls to fib at once.a

We can use getline
```c
ssize_t getline(char **lineptr, size_t *n, FILE *stream);
```
It 
- Allocates memory for us (`lineptr`), but we have to remember to free.
- Returns the size of the line and stores the size of the memory underlying it in `n`

```c
int main(void) {
  char *line = NULL;
  ssize_t line_len;
  size_t buf_len = 0;
  while ((line_len = getline(&line, &buf_len, stdin)) != 0) {
    int num = atoi(line);
    printf("fib(%d) = %d\n", num, fib(num));
  }
  free(line);
}
```

```bash
$ ./fibs < fib_inputs.txt
```

**Making it threaded**
We want to ensure that there are no race conditions of two threads calculating fib of the same number.
We could do one thread per line.

```c
void* fib_thread(void* arg) {
char *line = arg;*
}
```


## Exercises 2024-10-07

### *Performance comparisons*

*Suppose you wish to parallelise an image processing algorithm. You are given an efficient sequential implementation I_s, and you then develop a parallel implementation I_p. You have access to two machines:*

1. *A machine with a single very fast processor running at 4GHz (i.e. executes 4 billion instructions per second).*
    
2. *A machine with eight slower processors, each running at 2.5GHz (i.e. each executes 2.5 billion instructions per second).*
    

*Explain how you could perform a fair performance comparison of I_s and I_p.*

When we compare the performance of two programs, both must be shown in their best light. _I_s_ cannot take advantage of more than one processor, so it would be unfair to use a single one of the slow processors. Conversely, parallel programs like _I_p_ might not run optimally on a single processor. Thus, we need to run _I_s_ on system 1 and _I_p_ on system 2 and compare their performance. Since the systems are different, we cannot use speedup-in-latency. What we need to do is compute the _throughput_ of the two programs, and then we can compute the speedup-in-throughput.