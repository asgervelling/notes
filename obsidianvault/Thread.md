A single thread of execution in a [[Process]].

![[Pasted image 20231011140238.png]]
##### Consists of
- Its own [[Logical Control Flow]]
- Its own stack

##### Threads share
- open  files
- the same virtual memory space

They are peers, there is no main thread.

**Unlike processes**, threads can interact with each other using their shared memory. Even unintentionally.
Switching to a different thread in the same process does not require switching to a new virtual address space.