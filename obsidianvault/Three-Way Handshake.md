The three-way handshake is a handshake mechanism described in the [[TCP]] protocol used for establishing communication between a client and a server.


**1. SYN (Synchronize)**
- The client sends a TCP packet with the SYN (Synchronize) flag set to 1.
- The packet also includes a sequence number, which is a randomly generated value chosen by the client.
- The client's SYN packet does not contain any data payload.

**2. SYN-ACK (Synchronize-Acknowledge)**
- Upon receiving the SYN packet from the client, the server responds with a TCP packet.
- The server sets the SYN and ACK (Acknowledgment) flags to 1 in this packet.
- The packet includes an acknowledgment number, which is the client's sequence number incremented by 1.
- The server also generates its own sequence number, which is a randomly chosen value.
- The SYN-ACK packet does not contain any data payload.

3. **ACK (Acknowledge)**
- After receiving the SYN-ACK packet from the server, the client sends another TCP packet.
- The client sets the ACK flag to 1 and includes the acknowledgment number, which is the server's sequence number incremented by 1.
- The client may also include data payload in this packet if it wants to send initial data along with the acknowledgment.

In summary, the three-way handshake involves the exchange of TCP packets between the client and server. The client initiates the connection by sending a SYN packet, the server responds with a SYN-ACK packet, and finally, the client acknowledges the server's response with an ACK packet. The sequence and acknowledgment numbers are used to establish a reliable connection and ensure that both devices are in sync before data transmission begins.