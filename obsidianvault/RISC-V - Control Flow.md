The program counter (PC) is a special [[RISC-V - Registers|register]] that contains the address of the current instruction in memory.

![[Pasted image 20250105163142.png]]

- Every non-jump instruction implicitly increments PC by 4.

## Conditional Jumps

**What makes computers interesting is their ability to make decisions based on data.**
- [[Machine Code|Machine code]] does not support structured control flow (if, while, for, etc).
- Instead we have to do conditional jumps.

![[Pasted image 20250105163336.png]]

## Loops
Loops are implemented by jumping backwards.

- Suppose we want to implement `c = a * b`. Assume:
  - *c* is in register *a0*.
  - *a* is in register *a1*.
  - *b* is in register *a2*.
- We'll implement it by repeated addition of *a* (*b* times).

```
addi a0, a1, 0      # Initialize a0 = a1
LOOP:               # Loop label
beq a2, zero, END   # Jump to end if no iterations left
addi a2, a2, -1     # Decrement b
addi a0, a0, a1     # Add a to c
jal zero, LOOP      # Try again
END:                # Loop end
```

Is this correct for all *a* and *b*?
- No. If *b* is negative, the program never ends.

### Fibonacci Numbers
```
                       # addi a0, zero, 10    # n
int n = 10;            # addi t0, zero, 1     # a
int a = 1;             # addi t1, zero, 1     # b
int b = 1;             # addi t2, zero, 1     # i
int i = 0;             # LOOP:
while (n != 0) {       # beq a0, zero, DONE
  out[i] = b;          # addi a0, a0, -1
  i = i + 1;           # sw t1, 0(t2)
  int tmp = a + b;     # addi t2, t2, 4
  a = b;               # add t3, t0, t1
  b = tmp;             # add t0, zero, t1
  n = n - 1;           # add t1, zero, t3
}                      # jal zero, LOOP
                       # DONE:
```

