DNS is a system for resolving domain names to IP addresses. 

DNS is hierarchical, just like the internet.
DNS is fault tolerant, since it is a distributed systems with redundant data. Multiple name servers can have the same DNS records, so there is a smaller risk of having a single point of failure.

### DNS Records
They're like a phone book

| Domain Name | IP Address |
| --- | --- |
| 72.441.23.43 | google.com |
| 921.3.91.91 | ebay.com |

### Recursive Resolver
*AKA name server or DNS resolver*.
A DNS resolver, also known as a recursive resolver, is a server that can both process DNS queries and maintain a cache of DNS records.
The cache is populated from an authoritative name server.
Each DNS record has a time to live (TTL), after which the record will be dropped from the cache. At that point, the recursive resolver will have to ask the authoritative name server for the record again upon the next DNS query.

### Authoritative Name Server
*Example domain: `ns54.worldnic.com`.*
Authoritative name servers are responsible for knowing everything about the domain, which includes the IP address. It is the source of truth for a set of DNS records.

### Root Server
The root server is at the top of the DNS hierarchy. There are 13 sets of these root servers strategically placed around the world. They are operated by 12 different organizations. Each set of root servers has its own IP address.

### TLD Server
These servers handle the top-level domain information (like .com, .org, .net). They don't have the IP address for a specific domain, but they have information about authoritative name servers responsible for the domains within their TLD.


### Example: Finding the google.com IP
![[Pasted image 20231102155603.png]]
- The user's browser sends a query for google.com to a recursive resolver (name server).
  - If the resolver server has cached google.com, the IP address is retrieved. Otherwise...
- The query will be sent to a root server. The root server does not know the IP address of google.com. Instead, it redirects the resolver to a TLD server for the .com TLD.
- The resolver will now ask the TLD server for the IP address of google.com.
- The TLD server does not know the IP address. Instead, it redirects the resolver to an **authoritative name server**. 
- The authoritative name server responds to the name server with the IP address for google.com.
- Finally, the name server responds to the user's local machine with the IP address initially asked for.
