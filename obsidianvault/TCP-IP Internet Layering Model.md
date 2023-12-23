### TCP/IP Model
![[Pasted image 20231119215457.png]]
The TCP/IP model differs from the OSI model, in that it has only four layers instead of seven.
Here is what the layers in TCP/IP refer to in OSI.

Here are the four layers with protocols:
![[Pasted image 20231119221018.png]]

##### Transport Layer
- Logical Communication between processes
  - Sender divides messages into segments.
  - Receiver re-assembles messages into segments.

- Principles underlying transport-layer services
  - (De)[[Multiplexing|multiplexing]].
  - Detecting corruption.
  - Optional: Reliable delivery, Flow control, Congestion control

- Transport-layer protocols in the Internet
  - [[User Datagram Protocol (UDP)|UDP]]
    - Simple, unreliable message delivery
  - [[TCP]]
    - Reliable bidirectional stream of bytes