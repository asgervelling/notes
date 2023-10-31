A lock is a mechanism that prevents a shared resource from being accessed by multiple [[Thread|threads]] or [[Process|processes]].

##### Types of Locks
- **[[Mutex]]** (very common)
  Short for mutual exclusion, a mutex is a lock that only one thread can acquire at a time. If a thread attempts to acquire a mutex that is already held by another thread, it will be blocked.
- **Semaphore**
  A semaphore is a more generalized form of a lock that allows a specified number of threads to enter the critical section simultaneously. It can be used to control access to a resource based on availability.
- **Read-Write Lock**
  This lock allows multiple threads to read a shared resource simultaneously, but only one thread can write to the resource at a time. It's useful when the majority of operations involve reading, and occasional writing.
