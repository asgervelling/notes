In order to help with scalability and availability, we have two types of [[Domain Name System|DNS]] queries.

### Recursive
- DNS servers take responsibility for resolution. In other words, the DNS client demands an IP address from the DNS server.
- DNS servers will contact other name servers as required
- Offloads work from the client.

### Iterative
- DNS Server responds with the best information it has
  - From cache or another server that may help
- Will not contact other DNS servers.
- Offloads work from the root and TLD servers.

	For example, a root servers and TLD servers typically give an iterative response, pointing to the DNS server that holds the next information. A root server knows the address of the right TLD server, which knows the address of the right authoritative name server.