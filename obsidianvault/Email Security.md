In email [[IT-Security|security]], SPF, DKIM and DMARC are tools that can help prevent [[Spoofing|spoofing]] and validate email authenticity.

### Sender Policy Framework (SPF)
SPF lets you specify the mail servers that are authorized to send emails from your domain.
It helps identify fake emails (AKA. spear phishing emails) that appear to be coming from your organization.

It is a [[Domain Name System (DNS)|DNS]] record you set on the sending organization's DNS server.
It may look like this:

```
example.com     text =
        "v=spf1 ip4:192.168.100.11 -all"
```

Meaning
- `v=spf1` - Use SPF version 1
- `ip4:192.168.100.11`: The IPv4 address that is allowed to send on behalf of `example.com`
- `-all`: Tells the receiver’s server that the addresses not listed in this SPF record are unauthorized to send any email. It also tells the servers to reject such addresses, as they do not come from the authentic sender.
  You can also specify other options such as `~all`, which doesn't reject unauthenticated emails but simply marks them as suspicious.

Receiving or rejecting emails based on SPF records works like this:
- The sending organization sets up SPF records that tell the world what email addresses are truly theirs.
- When the sending email server sends an email to the receiving email server, that receiving email server checks the SPF record. If not authentic, it is marked as spam.

![[Pasted image 20231108124558.png]]


### DomainKeys Identified Mail Standard (DKIM)
DKIM was created for the same purpose as SPF: To prevent spammers from impersonating you as an email sender.

DKIM is also a DNS record. Here is an example:
![[Pasted image 20231108132146.png]]


With DKIM, you sign your emails with a unique [[Digital Signature|signature]], letting the receiver verify that the sender was really you, and that the message was not changed (tempered with) after you sent it.

You use a private key. The public key is published to the DNS server.

![[Pasted image 20231108131447.png]]


### Domain-based Message Authentication, Reporting and Conformance (DMARC)
DMARC allows domain owners to tell receiving mail servers what to do with the incoming emails that failed SPF or DKIM authorization.

DMARC is a DNS record.

The flow is like this:
- The sending email server sends an email
- Receiving email server receives it and
  - Does an SPF check to see if the email is coming from a trusted email server
  - Does a DKIM check to verify that the public and private keys match
- Finally, it checks the DMARC record to see what to do with records that didn't pass the SPF check, the DKIM check or both.
  - Emails may be quarantined or rejected
  - DMARC can also be configured to send back a report to the sending email server. These are called forensic reports and aggregate reports

![[Pasted image 20231108132824.png]]


### Questions
> *A few years ago, it was discovered that some organisations had set up DKIM in such a way that emails were signed with 512-bit RSA keys. Such keys were explicitly allowed in the DKIM standard. Explain why this is a problem.*

With rapidly increasing processing power of computers, RSA keys with a 512-bit length, previously considered to be secure, can be cracked in a short period of time. Today, a minimum of 1024 bit RSA should be used. Organizations like the American National Institute of Standards and Technology (NIST) go further, and recommend a minimum of 2048 bits. [^1]


> *With SPF, how much is the risk of spear phishing lowered, if at all?*




[^1]: https://hackerone.com/reports/550937#:~:text=Length%20of%20the%20Key,bit%20RSA%20should%20be%20used.