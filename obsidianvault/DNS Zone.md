A [[Domain Name System|DNS]] zone is a section of a domain name space that a certain administrator has been delegated control over.
DNS zones allow a domain name space to be divided into different sections.

If `example.com` was broken into three subdomains, such as
`shop.example.com`, `blog.example.com` and `support.example.com`,
the head administrator could create DNS zones and delegate control over these subdomains to other administrators.

Let's say that `shop.example.com`, `blog.example.com` only employ a few computers, and `support.example.com` uses many. The head administrator could create two zones out of the three subdomains, and assign each zone to an administrator

![[Pasted image 20231102170153.png]]

DNS zones are created for manageability purposes.
Each DNS zone will have their own DNS zone file, which contains an SOA [[DNS Record|record]].