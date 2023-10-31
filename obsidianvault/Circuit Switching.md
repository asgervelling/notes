> There are two fundamental approaches to moving data through a network of links and switches: Circuit switching and packet switching.
> In circuit switching, as opposed to [[Packet Switching]], the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are *reserved* for the duration of the communication session between the end systems.

Using a restaurant as an analogy, we can view circuit switching as a type of restaurant that requires reservations. We have to go through the trouble of calling and making a reservation. That means, however, that when we get to the restaurant, we can be seated immediately (the greeter at the restaurant is a router).
Using packet switching, we could just go to the restaurant, but we might have to wait for a table once we get there (wait in the output queue).
Another example of circuit switching is telephony.

![[Pasted image 20231023112511.png]]
The figure above illustrates a circuit switched network. In this network, the four circuit switches are interconnected by four links. Each of these links has four circuits, so that each link can support four simultaneous connections. The hosts are each directly connected to one of the switches.
Because each link has four circuits, the connection gets one fourth of the link's total transmission capacity for the duration of the connection.
For example, if each link between adjacent switches has a transmission rate of 1 Mbps, then each end-to-end circuit-switch connection gets 250 kbps of dedicated transmission rate.

### Multiplexing in circuit-switched networks
A circuit in a link is implemented with either frequency-division multiplexing (FDM) or time-division multiplexing (TDM).

##### Frequency-division multiplexing (FDM)
With FDM, the frequency spectrum of a link is divided up among the connections established across the link. 
The link dedicates a frequency band to each connection. In telephone networks, this band usually has a width of 4 kHz (4000 hertz or 4000 cycles per second). The width of the band is called **bandwidth**.
Think of a radio station that shares the frequency spectrum with other radio stations. They all get a specific frequency band in the spectrum between 88 MHz and 108 MHz.

##### Time-division multiplexing (TDM)
For a TDM link, time is divided into frames of fixed duration, and each frame is divided into a fixed number of time slots. When the network establishes a connection across a link, the network dedicates one time slot in every frame to this connection.

The transmission rate of a circuit using TDM is equal to the frame rate multiplied by the number of bits in a slot. Example:
- The link transmits 8000 frames per second
- Each slot consists of 8 bits
- Transmission rate of each circuit is 64 kbps.


![[Pasted image 20231023113409.png]]
The figure above illustrates FDM and TDM for a specific network link supporting up to four circuits.
For FDM, the frequency domain is segmented into four bands, each of bandwidth 4 kHz.
For TDM, the time domain is segmented into frames, with four time slots in each frame - one for each circuit.


##### Example: Sending a file
- File size: 640,000 bits
- Sent from host A to host B over a circuit-switched network.
- All links use TDM with 24 slots and a bit rate of 1.536 Mbps.
- It takes 500 ms to establish an end-to-end circuit before Host A can begin transmitting the file.
How long does it take to send the file?

Each circuit has a transmission rate of $1.536\ \textrm{Mbps}/24 = 64 \textrm{kbps}$, so it takes $\frac{640,000\ \textrm{bits}}{64\ \textrm{kbps}} = 10s$ to transmit the file. To this 10 seconds, we add the circuit establishment time, giving 10.5 seconds to send the file.

##### Exercise (Computer Networks, Ch. 1, R13)
*Suppose users share a 2 Mbps link.
Also suppose each user transmits continuously at 1 Mbps when transmitting, but each user transmits only 20 percent of the time.*
1. *When circuit switching is used, how many users can be supported?*
   Two users ($2\ \textrm{Mbps} / 1\ \textrm{Mbps} = 2$).
2. *For the remainder of this exercise, suppose packet switching is used.*
	See answer in [[Packet Switching]].
