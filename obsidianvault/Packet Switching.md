> Between source and destination, each packet travels through communication links and **packet switches** (for which there are two predominant types, [[Router|routers]] and link-layer switches). Packets are transmitted over each communication link at a rate equal to the full transmission rate of the link. So, if a source end system or a packet switch is sending a packet of *L* bits over a link with a transmission rate *R* bits/s, then the time to transmit the packet is *L/R* seconds.

##### Store-and-Forward Transmission
Most packet switches use store-and-forward transmission at the inputs to the links. This means that the packet switch must receive the entire packet before it can begin to transmit the first bit of the packet onto the outbound link.
![[Pasted image 20231023100349.png]]


##### End-to-end delay
Sending one packet from source to destination over a path consisting of $N$ links, each of rate $R$ (thus there are $N-1$ routers), the end-to-end delay is
$$
d_{\mathrm{end-to-end}} = N\frac{L}{R}
$$

##### Example: Time to transmit a packet, as a timeline
- At time 0, the source begins to transmit
- At time *L/R* seconds, the source has transmitted the entire packet, and the entire packet has been received and stored at the router (not accounting for propagation delay). At this time, the router can begin to transmit the packet onto the outbound link towards the destination.
- At time $2L/R$ seconds, the router has transmitted the entire packet, and the entire packet has been received by the destination.

Thus, the total delay is $2L/R$. If the packet switch didn't need to store (buffer) the entire packet before forwarding it, the delay would only be *L/R*. 

##### Example: Time to transmit three packets, as a timeline
- $t_0$: Src send $P_1$ to PS (The source starts sending packet 1 to the packet switch)
- $t_{L/R}$: PS recv $P_1$ (The packet switch has received packet 1).
	    PS send $P_1$ to dest (The packet switch starts sending packet 1 to the destination)
	    Src send $P_2$ to PS
- $t_{2L/R}$: Dest recv $P_1$
	     PS recv $P_2$
	     PS send $P_2$ to dest
	     Src send $P_3$ to PS
 - $t_{3L/R}$: Dest recv $P_2$
	      PS recv $P_3$
	      PS send $P_3$ to dest
  - $t_{4L/R}$: Dest recv $P_3$



##### Queuing delays and packet loss
Each packet switch has multiple links attached to it. For each attached link, the packet switch has an output buffer (AKA. output queue), which stores the packets that the router is about to send into that link.
If a packet arrives to a packet switch with that is busy transmitting another packet, it must wait in the output queue.
If a packet arrives to a packet switch with a completely full output queue, that packet is dropped (lost). 

##### Forwarding tables and routing protocols
Imagine you're driving from your town to your friends town, some few hundred kilometers away. You haven't visited him before, so you don't know the way.
You ask someone if they know where the right highway is. You then drive along it and see a sign that leads to your friends town. In your friend's town, you ask someone for your friend's neighborhood. In your friend's neighborhood, you ask for his street. Finally, you use the house number, your friend gave you, to locate his house.

That is the way a packet gets from its source to its destination via links. To do this, it uses something known as a forwarding table.
Each packet has a destination IP address. The routers use that destination address, or a part of it, to match with the addresses in their forwarding table.

But how do those addresses get set?
Using routing protocols. These can determine shortest paths from each router to each destination, and use the shortest path results to configure the forwarding tables in the routers.


##### Exercise (Computer Networks, Ch. 1, R11)
*Suppose there is exactly one packet switch between a sending host and a receiving host. The transmission rates between the sending host and the switch and between the switch and the receiving host are $R_1$ and $R_2$, respectively. Assuming that the switch uses store-and-forward packet switching, what is the total end-to-end delay to send a packet of length $L$? Ignore queuing, propagation delay and processing delay.)*

$$
d_{end-to-end} = \frac{L/R_1}{L/2}
$$
It takes $L/R_1$ seconds to send L bits at $R_1$ bps. It takes $L/R_2$ seconds to send L bits at $R_2$ bps.


##### Exercise (Computer Networks, Ch. 1, R13)
*Suppose users share a 2 Mbps link.
Also suppose each user transmits continuously at 1 Mbps when transmitting, but each user transmits only 20 percent of the time.*
1. *When circuit switching is used, how many users can be supported?*
   Two users ($2\ \textrm{Mbps} / 1\ \textrm{Mbps} = 2$). Half of the bandwidth for each user.
2. *For the remainder of this exercise, suppose packet switching is used. Why will there essentially be no queuing delay before the link if two or fewer users transmit at the same time? Why will there be a queuing delay if three users transmit at the same time?*
   There is enough bandwidth on the output queue (size of 2 Mb) for two users who are transmitting at 1 Mbps. They will completely fill up the queue, so those two have just enough space.
   If we try to add a third user, he will have to wait or maybe lose packets. There is not enough room in the queue.
3. *Find the probability that a given user is transmitting.*
   0.2. It says right there in the text.
4. *Suppose now there are three users. Find the probability that at any given time, all three users are transmitting simultaneously. Find the fraction of time during which the queue grows.*
   Probability that three users transmit at the same time: $0.2^3 = 0.008$. 

##### Exercise (Computer Networks, Ch. 1, R14)
