## CompSys - A4
##### Original Work
This submission is the work of the indicated group members, and only the indicated group members. If ChatGPT, or an equivalent system, has been used at any stage of the report writing or code construction it has been clearly indicated.

### Programming Part

##### Implementation
- To get started, focus on the register request.
- Then, implement replying to any command with dummy text such as "Served hamlet.txt". Don't actually implement file retrieval yet.
- Implement the inform command.
- Run a few peers that inform each other of their existence. Send a message and see the dummy text appear in every terminal.
- Start implementing small file retrieval
- Assert that it works, perhaps with a unit test. If it is too cumbersome, don't waste effort on it.
- Start implementing block retrieval

> For A4 including (but not limited to) we need to hear how the peer list is handled in every case it is written to. As well as anything else that was non-obvious or implementation details where the design was open ended

*-From discord*. 

##### Thread Safety
Which resources are shared between client and server within the peer?
- The file being served or requested. We have to make sure it is not being accessed in read and write mode at the same time.
- If there are no cycles in the resource allocation graph, there will be no deadlock.
- If there is a cycle in the resource allocation graph, there may be a deadlock.
  - "Holding" refers to an assignment edge. A process is currently operating on a resource.
  - "Waiting for" refers to a request edge. A process is waiting to acquire a resource.
- If there is a cycle in the resource allocation graph, use allocation, request and availability matrices https://www.youtube.com/watch?v=qPuf5B5xPCs

##### Testing
- Write tests that gets each of the five status codes.


