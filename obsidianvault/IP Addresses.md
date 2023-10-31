IPv4 addresses are 32 bits.
Can be used in C because they are 32 bit numbers.

`0x8002C2F2 = 128.2.194.242`

In C, you can store an IP address in an address struct
```C
/* Internet address structure */
struct in_addr {
  uint32_t s_addr; /* network byte order (big-endian) */
};
```
