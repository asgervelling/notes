##### Joining Threads

- **Overview**: Joining threads is a synchronization mechanism where one thread waits for another thread to complete its execution before proceeding. It allows for sequential flow in a multi-threaded program, ensuring certain operations are completed before others.

##### Cthreads Functions:

- **`pthread_create`**: Creates a new thread.
- **`pthread_join`**: Waits for a specific thread to complete execution.
- **`pthread_exit`**: Terminates the calling thread and returns a value.
- **`pthread_cancel`**: Requests cancellation of a thread.

##### Example C Program:
```c
#include <pthread.h>
#include <stdio.h>

void* thread_function(void* arg) {
  printf("Thread is running\n");
  return NULL;
}

int main() {
  pthread_t thread_id;
  int ret;

  // Create a new thread
  ret = pthread_create(&thread_id, NULL, thread_function, NULL);
  if (ret) {
    fprintf(stderr, "Error - pthread_create() return code: %d\n", ret);
    return 1;
  }

  // Wait for the thread to complete
  pthread_join(thread_id, NULL);
  printf("Thread has completed.\n");
  return 0;
}
```

This program creates a new thread using `pthread_create` and waits for it to complete using `pthread_join`. The new thread executes `thread_function` and prints a message. The main thread waits for this execution to complete before proceeding further.
