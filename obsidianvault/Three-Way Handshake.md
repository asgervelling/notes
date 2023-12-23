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
- Can also respond with NACK (not acknowledged), meaning "could you repeat that"?

In summary, the three-way handshake involves the exchange of TCP packets between the client and server. The client initiates the connection by sending a SYN packet, the server responds with a SYN-ACK packet, and finally, the client acknowledges the server's response with an ACK packet. The sequence and acknowledgment numbers are used to establish a reliable connection and ensure that both devices are in sync before data transmission begins.

### Reliable delivery

- **Detect bit errors:** Checksum
- **Detect missing data**: Sequence number
  - Used to detect a gap in the stream of bytes, and for putting the data back in order.
- **Recover from lost data**: re-transmission
  - Sender re-transmits lost or corrupted data
  - Two main ways to detect lost packets

### In-depth

##### Ephemeral port
The first thing you see if you open up Wireshark and look at the first SYN packet, you will see a high `src port`, such as `54846`. 
Assigned automatically by client kernel when client makes a connection request.

##### Stream index
In the first ACK packet, you will see a stream index, which is a four-tuple - meaning two IP addresses and two port numbers.

##### Sequence numbers
The first sequence number can be quite high. It is unlikely that anyone will guess it.
Because humans have a hard time keeping track of all those high numbers, Wireshark shows you a relative sequence number. That will be either 0 or 1 instead of 584359 or 584360.

##### Acknowledgments from receiver
- Positive: “okay” or “uh huh” or “ACK”
- Negative: “please repeat that” or “NACK”

##### Re-transmission by the sender
Re-transmission by the sender if
- They did not receive an ACK
- They received a NACK

##### Timeout by the sender (“stop and wait”)
Don’t wait forever without some acknowledgment.
Part of [[Automatic Repeat Request (ARQ)]].

