[[Lock|Locks]] are not the only primitives used to build [[Concurrency|concurrent]] programs.
Condition variables can be used to signal that a thread has finished doing its work.

### How it works
A condition variable is a primitive a thread can use. A [[Thread|thread]] can use a condition variable to do two things:
- Put itself to sleep with `wait()`. 
- Send a `signal()` to the threads waiting for the condition variable, that it the condition is now met.

### The problem we're trying to solve
Consider this example of a parent process waiting for a child process to finish before doing its own work:

```
bool finished = false;

fn child():
  print("Child")
  finished = true

fn main():
  print("Parent")
  start_process(child)
  while not finished:
    do nothing
  // Child process is finished
  print("Finished")
```

This is really inefficient, since the parent process wastes clock cycles spinning (doing a while loop).

### How condition variables solve it
Condition variables are not boolean variables that you set. That would make you dependent on spinning, which is wasteful. Condition variables are signals. 
Under the hood, it is a queue that threads can put themselves on.


### The C functions
```C
pthread_cond_wait(pthread_cond_t *c, pthread_mutex_t *m);
pthread_cond_signal(pthread_cond_t *c);
```
