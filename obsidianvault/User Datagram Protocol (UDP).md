Simple, unreliable message delivery.

![[Pasted image 20231119224646.png]]

### Why would anyone use UDP?
- Fine control over what data is sent and when
  - As soon as app process writes into socket, UDP will package data and send packet 

- No delay for connection establishment 
  - UDP blasts away without any formal preliminaries, avoids introducing unnecessary delays

- No connection state (no buffers, sequence #’s, etc.)
  - Can scale to more active clients at once
  - Small packet header overhead (header only 8B long)

### [[Socket]] programming with UDP
![[Pasted image 20231119225141.png]]
where

```C
ssize_t recvfrom(int sockfd,
				 void* buff,
				 size_t nbytes,
				 int flags,
				 struct sockaddr* from,
				 socklen_t *addrlen);
```

```C
ssize_t sendto(int sockfd,
			   const void *buff,
			   size_t nbytes,
			   int flags,
			   const struct sockaddr *to,
			   socklen_t addrlen);
```

