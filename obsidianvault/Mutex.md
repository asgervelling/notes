Short for mutual exclusion, a mutex is a [[Lock|lock]] that only one thread can acquire at a time. If a thread attempts to acquire a mutex that is already held by another thread, it will be blocked.

### Two operations
Lock and unlock (AKA. claim and release / P and V).

#### Mutexes in C
We have four functions for dealing with mutexes in C.

1. **`pthread_mutex_init`**: This function initializes a mutex. It takes a pointer to a `pthread_mutex_t` structure, which will hold the mutex attributes.
   ```c
   int pthread_mutex_init(pthread_mutex_t *mutex,
						  const pthread_mutexattr_t *attr);
   ```
  - `mutex`: Pointer to the mutex structure to be initialized.
  - `attr`: Pointer to the attributes for the mutex. If NULL is passed, default attributes are used.

2. **`pthread_mutex_lock`**: This function locks a mutex. If the mutex is already locked by another thread, the current thread will block until it can obtain the lock.
   ```c
   int pthread_mutex_lock(pthread_mutex_t *mutex);
   ```
   

3. **`pthread_mutex_unlock`**: This function unlocks a mutex. If any threads are waiting for this mutex, one of them will be allowed to acquire the lock.
   ```c
   int pthread_mutex_unlock(pthread_mutex_t *mutex);`
   ```


4. **`pthread_mutex_destroy`**: This function destroys a mutex, releasing any resources associated with it.
   ```c
   int pthread_mutex_destroy(pthread_mutex_t *mutex);
   ```

### Initializing a Mutex
> With `POSIX` threads, there are two ways to initialize locks. One way to do this is to use `PTHREAD_MUTEX_INITIALIZER`, as follows: `pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;`
\ 
> Doing so sets the lock to the default values and thus makes the lock usable. The dynamic way to do it (i.e., at run time) is to make a call to `pthread_mutex_init()` as follows: `int rc = pthread_mutex_init(&lock, NULL); assert(rc == 0); // always check success!`
\ 
> The first argument to this routine is the address of the lock itself, whereas the second is an optional set of attributes. Read more about the attributes yourself; passing NULL in simply uses the defaults. **Either way works**, but we usually use the dynamic (latter) method.

Source: *OSTEP*.

### Example Use
   ```c
#include <pthread.h>
#include <stdio.h>

pthread_mutex_t myMutex;

void* threadFunction(void* arg) {
    pthread_mutex_lock(&myMutex);  // Acquire the lock

    // Critical section (protected by the mutex)
    printf("Thread %ld in the critical section.\n", (long)arg);

    pthread_mutex_unlock(&myMutex);  // Release the lock

    return NULL;
}

int main() {
    pthread_t thread1, thread2;

    // Initialize the mutex
    pthread_mutex_init(&myMutex, NULL);

    // Create threads
    pthread_create(&thread1, NULL, threadFunction, (void*)1);
    pthread_create(&thread2, NULL, threadFunction, (void*)2);

    // Join threads (wait for them to finish)
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    // Destroy the mutex
    pthread_mutex_destroy(&myMutex);

    return 0;
}
   ```
   