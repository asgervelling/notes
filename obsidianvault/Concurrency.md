When multiple things happen at once.
From a programming perspective, concurrency means having multiple [[Logical Control Flow]]s executing simultaneously.
That's what the word means, anyway. In computing, there is a difference between concurrency and [[Parallelism]].
A concurrent program will multitask really fast, like a human: Do a bit of work on [[Thread]] A, then thread B, then thread A, etc. In computing, this is called interleaving.
[[Process]]es and [[Thread]]s are examples of concurrent execution.

##### Why we use concurrency
We want to make our programs faster, either measured by [[Latency]] or [[Throughput]].
We use the ideas of [[Scaling]]. An exam question might be "Is this function scalable?""
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
