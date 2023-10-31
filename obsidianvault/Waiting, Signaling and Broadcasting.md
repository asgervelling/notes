Waiting, signaling, and broadcasting are fundamental mechanisms used for synchronization:

- **Waiting**: Threads can wait for a specific condition to be met using condition variables. When the condition is not met, a thread can go to sleep, conserving system resources.
  
- **Signaling**: Threads can signal other threads when a condition they are waiting for is met. This awakens the waiting thread, allowing it to recheck the condition and proceed accordingly.
  
- **Broadcasting**: Threads can broadcast a signal to wake up multiple waiting threads simultaneously when a condition they are waiting for becomes true.

## C Functions for Synchronization
The POSIX threads library (`pthreads`) in C provides functions to achieve thread synchronization using condition variables:

- `pthread_cond_wait(cond, mutex)`: Puts the calling thread to sleep and releases the mutex. It wakes up when another thread signals the condition, and the mutex is reacquired.
  
- `pthread_cond_signal(cond)`: Wakes up at least one thread waiting on the specified condition variable.
  
- `pthread_cond_broadcast(cond)`: Wakes up all threads waiting on the specified condition variable.

## Example: Producer-Consumer Scenario
In this example, we demonstrate a simple producer-consumer scenario using a shared buffer and condition variables:

```c
#include <pthread.h>
#include <stdio.h>

pthread_cond_t myCondition = PTHREAD_COND_INITIALIZER;  // Initialize statically
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;  // Initialize statically
int isFull = 1;  // Initial state: full

void* consumer(void* arg) {
    pthread_mutex_lock(&mutex);

    while (isFull) {
        printf("Buffer is full. Consumer is waiting...\n");
        pthread_cond_wait(&myCondition, &mutex);
    }

    printf("Consumed data.\n");
    
    pthread_mutex_unlock(&mutex);

    return NULL;
}

void* producer(void* arg) {
    // Produce data...

    pthread_mutex_lock(&mutex);

    // Update the state (e.g., buffer is not full)
    isFull = 0;

    // Signal the condition
    pthread_cond_signal(&myCondition);

    pthread_mutex_unlock(&mutex);

    return NULL;
}

int main() {
  pthread_t producer_thread, consumer_thread;
  
  // Create consumer and producer threads
  pthread_create(&consumer_thread, NULL, consumer, NULL);
  pthread_create(&producer_thread, NULL, producer, NULL);
  
  // Join threads (wait for them to finish)
  pthread_join(consumer_thread, NULL);
  pthread_join(producer_thread, NULL);
  
  // Destroy mutex and condition variable
  pthread_mutex_destroy(&mutex);
  pthread_cond_destroy(&myCondition);
  return 0;
}
```

