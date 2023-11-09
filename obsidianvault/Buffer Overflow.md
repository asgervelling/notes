Buffer overflow occurs when you write more bytes to a buffer than you have allocated, such as

```C
int string[2];
string[0] = 0;
string[1] = 1;
string[2] = 2; // Buffer overflow
```

You have now corrupted your heap.
If the memory was allocated on the stack, the stack would have been corrupted.



##### Questions
> *Is an [[Internet Protocol Security (IPSec)|IPSec]] connection to your company network secure even if your home router’s TCP/IP implementation has a buffer overflow vulnerability?*

As IPsec is not providing a full end to end security, as the packets are unprotected after
going through the gateway to the enterprise network. The IPsec connection does not protect
your connection if your home has a buffer overflow vulnerability.