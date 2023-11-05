A mapping from a domain to an IP address, stored in a [[DNS Database]].

|Type|Domain Name|IP Address|TTL|
|---|---|---|---|
|A|72.441.23.43|google.com|7200|
|A|921.3.91.91|ebay.com|7200|

There are different types of DNS records.

### A Record
Resolves a domain name to an IPv4 address.

|Type|Domain Name|IP Address|TTL|
|---|---|---|---|
|A|72.441.23.43|google.com|7200|

### AAAA Record
Resolves a domain name to an IPv6 address.

|Type|Domain Name|IP Address|TTL|
|---|---|---|---|
|AAAA|2001:0000:130F:0000:0000:09C0:876A:130B|google.com|7200|


### CNAME Record
*Canonical name*.
Resolves a domain or subdomain to another domain.

|Type|Domain Name|IP Address|TTL|
|---|---|---|---|
|CNAME|`www.google.com`|google.com|7200|
|CNAME|`www.filesharing.example.com`|`ftp.example.com`|7200|

A canonical name record is a good way to create an alias for a domain name. It can be used to load balance a distributed system or to point to a service on a shared computer

### MX Record
*Mail Exchange record*.
Points to a server where email should be delivered for that domain name.

|Type|Priority|Domain Name|IP Address|TTL|
|---|---|---|---|---|
|MX|10|example.com|mail1.example.com|7200|

When you send an email to `bob@example.com`, the MTA (Mail Transfer Agent) will query the MX records because it is looking for an email server.

MX records will generally have two entries - a primary and a secondary email server.

|Type|Priority|Domain Name|IP Address|TTL|
|---|---|---|---|---|
|MX|10|example.com|mail1.example.com|7200|
|MX|20|example.com|mail2.example.com|7200|

A low number in priority means high priority. If the highest priority email server goes down, the email server with the second to highest priority will be used.

### SOA Record
*Start of Authority*.
Stores administrative information about a [[DNS Zone]].


|Type|MNAME|RNAME|SERIAL|RETRY|TTL|
|---|---|---|---|---|--|
|SOA|ns1.example.com|admin.example.com|549243|60|7200|

`MNAME`: Primary [[Domain Name System#Recursive Resolver|name server]].
`RNAME`: Email address of the administrator responsible for this zone.
			`@` becomes `.`, so the above administrator's email address is `admin@example.com`.
`SERIAL`: The serial number represents a version in the zone. Whenever an update happens in the zone, the serial number will change, which tells the secondary servers to update as well.

### NS Record
Provides the name of the [[Domain Name System#Authoritative Name Server|authoritative name server]] within a domain.
An NS record will typically list two authoritative name servers: A primary and a secondary.

### SRV Record
*Service record*.
Unlike the other records which only point to an IP address of a server, the service record will include a port number of a service on that server.

|TYPE|PRIORITY|SERVICE|PORT|NAME|WEIGHT|TTL|
|-|-|-|-|-|-|-|
|SRV|10|chat.example.com|12345|example.com|0|7200|

`PRIORITY`: A lower number in priority means high priority and more traffic to that server.
`WEIGHT`: A way to fine-tune the priority, if multiple servers have the same priority.

### PTR Record
*Pointer record*.
Allows for reverse DNS lookup - a way to determine the domain name associated with an IP address.

|TYPE|IP ADDRESS|NAME|TTL|
|-|-|-|-|
|PTR|12.34.56.78|example.com|7200|

Pointer records are sometimes used to prevent email spam.
Whenever an email is received, the email server uses the pointer record to check the sender's authenticity by matching the domain name in the email with its authentic IP address.

### TXT Record
The text record contains miscellaneous information about a domain.

|TYPE|NAME|VALUE|TTL|
|-|-|-|-|
|TXT|example.com|`k=rsa; p=Uk9A5HwKTL;`|7200|

Can provide authenticity by making sure the value comes from the authentic source.

### Breaking down a domain
![[Pasted image 20231102163827.png]]
There is an invisible root domain after the TLD. That's why we have root servers. We need a way to direct a DNS query to a TLD server.