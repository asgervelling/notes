The two main concepts are [[RISC-V - Registers|registers]] and [[RISC-V - Instructions|instructions]].

```
add x1, x2, x3
```

- *add* is the instruction type.
- *x1, x2, x3* are names of registers.
- Add the values in the registers *x2* and *x3* and put the result in *x1*.
- A register is basically a variable implemented in hardware.
- An instruction is basically a simple function implemented in hardware.
- RISC-V/32 exposes a fixed number of registers with a fixed size (32 bits).
- ...and a fixed number of instructions.


## From C to RISC-V
A [[Compiler|compiler]] must decide in which registers to store variables.

![[Pasted image 20250105151939.png]]

Assembly instructions are very simple, so high-level languages must break up complex expressions, which often require extra registers.

![[Pasted image 20250105152016.png]]

## Registers
See [[RISC-V - Registers]].

## Instructions
See [[RISC-V - Instructions]].


## Exercises 2024-09-04

### COD 2.1
*For the following C statement, write the corresponding RISC-V assembly code. Assume that the C variables f, g and h have already been placed in registers x5, x6 and x7 respectively. Use a minimal number of RISC-V assembly instructions.*

C code:
```
f = g + (h - 5);
```

RISC-V:
```
addi x7, x7, -5    # h = h - 5
add  x5, x6, x7    # f = g + h
```

### COD 2.2
*Write a single C statement that corresponds to the two RISC-V assembly instructions below.*

```
addi x7, x7, -5
add  x5, x6, x7
```

```
f = g + h
```

### COD 2.3
*For the following C statement, write the corresponding RISC-V assembly code. Assume that the variables f, g, h, i and j are assigned to registers x5, x6, x7, x28 and x29, respectively. Assume that the base address of the arrays A and B are in registers x10 and x11, respectively.*

```
B[8] = A[i-j];
```

```
sub  x5, x28, x29    # f = i - j
add  x5, x5, x10     # f = f + A
lb   x5, 0(x5)       # f = Mem[f]
sb   x5, 8(x11)      # Mem[B+8] = f
```

### COD 2.4
*Translate the RISC-V code to C.*

For reference:

| f   | g   | h   | i   | j   | A   | B   |
| --- | --- | --- | --- | --- | --- | --- |
| x5  | x6  | x7  | x28 | x29 | x10 | x11 |

```
slli  x30, x5,  2   # x30 = f * 4
add   x30, x10, x30 # x30 = &A[f]
slli  x31, x6,  2   # x31 = g * 4
add   x31, x11, x31 # x31 = &B[g]
lw    x5, 0(x30)    # f = A[f]

addi x12, x30, 4    # x12 = &A[f+1]    (since +4 bytes)
lw   x30, 0(x12)    # x30 = A[f+1]
add  x30, x30, x5   # x30 = A[f+1] + A[f]
sw   x30, 0(x31)    # B[g] = A[f+1] + A[f]
```

Answer:
```
B[g] = A[f+1] + A[f]
```

### COD 2.6
Translate `0xabcdef12` into decimal.
$$
(10 \cdot 16^7) + (11 \cdot 16^6) + (12 \cdot 16^5) + (13 \cdot 16^4) +
(14 \cdot 16^3) + (5 \cdot 16^2) + (1 \cdot 16^1) + (0 \cdot 16^0) \\
= 2882400018
$$

### COD 2.25
*Translate to RISC-V, and use a minimal number of instructions:*

```
for (i = 0; i < a; i++)
  for (j = 0; j < b; j++)
    D[4*j] = i + j;
```

*Assume that values are in these registers:
  a x5
  b x6
  i x7
  j x29
 D x10*

```
addi x7, x0, 0                # i = 0 + 0
addi x29, x0, 0               # j = 0 + 0

LOOPI:  bge  x7, x5, END      # if (i >= a) go to END
        addi x7, x7, 1        # i = i + 1

LOOPJ:  bge  x29, x6, LOOPI   # if (i >= b) go to LOOPI
        add  x30, x7, x29     # x30 = i + j
        slli x31, x29, 2      # x31 = j * 4
        add  x31, x10, x31    # x31 = &D[j * 4]
        sw   x30, 0(x31)      # D[j*4] = x30  => D[j*4] = i + j
        addi x29, x29, 1      # j++
        jal  x0, LOOPJ

END:


// Consider using four parts. The solution from DIKU:
LOOP0:  beq x7, x5, DONE0
        addi x29, x0, 0
LOOP1:  beq x29, x6, DONE1
        slli x30, x29, 2
        slli x30, x30, 2
        add x30, x10, x30
        add x31, x7, x29,
        sw x31 0(x30)
        addi x29, x29, 1
        jal x0, LOOP1
DONE1:
        addi x7, x7, 1
        jal x0, LOOP0
DONE0:
```

