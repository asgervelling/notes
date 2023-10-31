### Iterative Server
An iterative server can only handle one request and transaction at a time. Its server [[Socket|socket]] blocks until the transaction is finished.
![[Pasted image 20231030105537.png]]

### Concurrent Server
A [[Concurrency|concurrent]] server creates a child server [[Logical Control Flow|flow]] for every client.

##### Approaches to Writing Concurrent Servers
1. Process-based
   - Kernel automatically interleaves multiple logical flows
   - Each flow has its own private address space
2. Event-based
   - Programmer manually interleaves multiple logical flows
   - All flows share the same address space
   - Uses technique called I/O multiplexing
3. Kernel-based
   - Kernel automatically interleaves multiple logical flows
   - Each flow shares the same address space
   - Hybrid of of process-based and event-based

##### Example: Process-Based Concurrent Echo Server
```C
int main(int argc, char **argv) {
  int listenfd, connfd;
  socklen_t clientlen;
  struct sockaddr_storage clientaddr;
  listenfd = compsys_helper_listenfd(argv[1]);
  while (1) {
    clientlen = sizeof(struct sockaddr_storage);
    connfd = accept(listenfd, (SA*)&clientaddr, &clientlen);
    if (fork() == 0) {
      close(listenfd); /* Child closes its listening socket */
      echo(connfd); /* Child services client */
      close(connfd); /* Child closes connection with client */
      exit(0); /* Child exits */
    }
    close(connfd); /* Parent closes connected socket (important!) */
  }
}
```




Read more: https://github.com/diku-compSys/compSys-e2023-pub/tree/main/lectures/23-10-30_non-blocking_servers_and_intro_to_security