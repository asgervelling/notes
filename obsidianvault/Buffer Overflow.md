Buffer overflow occurs when you write more bytes to a buffer than you have allocated, such as

```C
int string[2];
string[0] = 0;
string[1] = 1;
string[2] = 2; // Buffer overflow
```

You have now corrupted your heap.
If the memory was allocated on the stack, the stack would have been corrupted.

