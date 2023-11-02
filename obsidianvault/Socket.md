A socket is an inter-process communication technique.
A socket is simply a [[File Types|type of file]] like any other. You can do IO on it.
To the kernel, a socket is an endpoint of communication.
To an application, a socket is a file descriptor that lets the application read/write from/to the network (recall that all Unix IO devices, including networks, are modeled as files.)
In a connection, there are listening file descriptors and connecting file descriptors.

A socket is an endpoint of a [[Internet Connection|connection]].
A socket address is an **IP address:port** pair. Essentially, [[Ports|ports]] are file descriptors. So the fact that we have 65000 or so ports, we are able to have 65000 open connections at the same time. In practice, however, we would run out of threads way before that.

You use one socket to send and one to receive, if it's two-way communication.

The main difference between regular file IO and socket IO is how the application "opens" the socket descriptors.

Sending a message is a blocking operation.

Short counts do occur. A short count is when the number returned by a `fread` or `read` is less than expected, i.e. the number of bytes read was less than expected.
You wanna read the header, to see how many bytes of the packet to read, but don't read way too much.

##### Socket Address Structures
**Generic**
```C
struct sockaddr {
	uint16_t sa_family; /* Protocol family */
	char sa_data[14]; /* Address data. */
};
```
[^1]
##### sockaddr_in
```C
#include <netinet/in.h>

struct sockaddr_in {
	short sin_family; // e.g. AF_INET
	unsigned short sin_port; // e.g. htons(3490)
	struct in_addr sin_addr; // see struct in_addr, below
	char sin_zero[8]; // zero this if you want to
};

struct in_addr {
	unsigned long s_addr; // load with inet_aton()
};
```


**IPv4 Socket**
```C
struct sockaddr_in { uint16_t sin_family; /* Protocol family (always AF_INET) */ uint16_t sin_port; /* Port num in network byte order */ struct in_addr sin_addr; /* IP addr in network byte order */ unsigned char sin_zero[8]; /* Pad to sizeof(struct sockaddr) */ };
```

##### Example: Setting up an address
```C
struct sockaddr_in serv_addr;
serv_addr.sin_family = AF_INET;
serv_addr.sin_port = htons(12345);
if (inet_pton(AF_INET, “123.123.123.123”, &serv_addr.sin_addr) <= 0) {  
	printf("Invalid address\n"); return -1;
}
```
##### Byte order
*Little-endian or big-endian.*
Use `inet_ntop`, `inet_pton` functions for converting between dotted decimal notation and IP addresses.
Use `htonl`, `htons` (host-to-network short), `ntohl` and `ntohs` functions for network byte order conversions.

##### AF_INET address family
This address family provides inter-process communication between processes that run on the same system or on different systems."

##### Protocol
`SOCK_STREAM`: Use [[Circuit Switching]] with [[TCP]].
`SOCK_DGRAM`: Use [[Packet Switching]] with [[UDP]].

##### Example: Echo server
![[Pasted image 20231025104642.png]]
```C
#include "compsys_helpers.h"

int main(int argc, char **argv) {
	int clientfd;
	char *host, *port, buf[MAXLINE];
	compsys_helper_state_t state;
	host = argv[1];
	port = argv[2];
	clientfd = compsys_helper_pen_clientfd(host, port);
	compsys_helper_readinitb(&state, clientfd);
	while (fgets(buf, MAXLINE, stdin) != NULL) {
		compsys_helpeer_writen(clientfd, buf, strlen(buf));
		compsys_helper_readlineb(&state, buf, MAXLINE);
		fputs(buf, stdout);
	}
	
	close(clientfd);
	exit(0);
}
```

For more examples on socket programming, see this [echo sever implementation](https://github.com/diku-compSys/compSys-e2023-pub/tree/main/lectures/23-10-25_network_programming/lecture_code).

##### Socket Interface
![[Pasted image 20231025112637.png]]

1. Bind
   `int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);`
2. Listen
   ``
3. Accept
   Servers wait for connection requests from clients by calling `accept`.
   `int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);`
4. Connect
5. Close


[^1]: https://www.gta.ufrj.br/ensino/eel878/sockets/sockaddr_inman.html