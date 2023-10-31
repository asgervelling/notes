A process is a, currently running, instance of an application.
It may use one or more [[Thread]]s to do so. If it only has one thread, that thread is called the main thread.

![[Pasted image 20231011140238.png]]
##### Consists of
A process typically consists of three parts
- A virtual memory space. Includes stack, code, heap, data, etc.
- A kernel context. Includes PID, open files, signal mask, parent PID, list of children, etc.
- An execution context. Has registers (including special ones like the program counter).

We can do [[Concurrency|concurrent]] programming either with multiple processes or multiple threads.