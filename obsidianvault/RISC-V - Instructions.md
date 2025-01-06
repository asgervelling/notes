Main article: [[RISC-V]].

- Instructions  operate on data stored in [[RISC-V - Registers|registers]].
- Load/store instructions ferry data between registers and [[Memory#Byte-Addressable Memory|byte-addressed memory]].
- Branch/jump instructions move the instruction pointer.
- Assembly is somewhat tedious but fundamentally simple.
- Hint: When you have to write an assembly program, consider first writing it in C-ish syntax with a single "instruction" per statement, and translate from there.

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

## Pseudo Instructions
An instruction allowed in the assembly syntax,but translated into one or more other instructions by the assembler.
- Think of them as shortcuts.
- Supported by some interpreters, but not implemented in hardware. They make it easier to write assembly code by hand.

![[Pasted image 20250105153427.png]]

