## Locality of Reference

- **Temporal Locality:** Accessing data that was accessed recently.
  ![[Pasted image 20250102114828.png]]
- **Spatial Locality:** Accessing data that is located close to recently accessed data.
  ![[Pasted image 20250102114839.png]]

*Definition of "close" depends on the exact form of storage, e.g. addresses for memory.*


## Memory Hierarchy
See [[Memory Hierarchy]].

## Byte-Addressable Memory
Memory can either be byte-addressable or word-addressable. Here, we take a look at when it is byte-addressable, as it can be quite intuitive.

- Registers are for scratch space; data is primarily stored in memory.
  ![[Pasted image 20250105153617.png]]
- Programs refer to data by address
  - Conceptually, envision as a large array of bytes.
     - It's not really, but it works as a semantic model.
  - An address is like an index into that array.
     - A pointer stores an address.
     - Addresses are ultimately just unsigned integers.
- System provides private address space to each [[Process|process]].
  
### The Von Neumann Bottleneck

![[Pasted image 20250105154023.png]]

- **Reading:** CPU sends address to memory.
  - Memory responds with contents at address.
- **Writing**: CPU sends address and data to memory.
  - Memory overwrites location with new contents.
- **The bus is slow!**

The distance between computation and storage is the main performance obstacle in most programs.

### Machine Words
Any given computer has a **word size**.

- "Native" size of integer-valued data.
  - But especially of memory addresses.
- 32-bit machines used to be the norm and are still found (e.g. [[RISC-V|RISC-V/32]]).
  $2^{32}$ different addresses, meaning 4 [[Byte Units|GiB]] can be addressed.
- 64-bit machines are most common.
  - $2^{64}$ different addresses, meaning 18 *EiB* can be addressed.
  - $18.4 * 10^{18}$ bytes.
  - Current machines only use lower 48 bits of address.
- Machines also support other data formats
  - Fractions or multiples of word size.
  - Always integral number of types.
  - Smaller types (e.g. 16-bit integers) take less space in memory, but are (usually) not faster than the "native" words.
  - But bigger types (e.g. 128-bit integers) are slower.
    
## Word-Addressable Memory

- Addresses specify byte locations
  - Address of first byte in word.
  - Addresses of successive words differ by 4 or 8 bytes (32 or 64 bits).
  - Addresses always refer to a byte even when addressing larger types (think of a pointer).

### Byte Ordering

- How are the bytes within a multi-byte word ordered in memory?
  - Most significant byte at lowest address, or least significant byte at lowest address?
- **Conventions**
  - [[Endianness (Byte Ordering)|Big endian]]: SPARC, POWER, Internet protocols.
  - Little endian: x86, ARM (mostly), [[RISC-V]].

## In RISC-V

```
byte Memory[M];
```

#### Main difference between registers and memory
Memory is dynamically addressable, while the [[RISC-V - Registers|registers]] we operate on are statically encoded in [[RISC-V - Instructions|instructions]].

- In a stored program computer, instructions are also just data stored in memory (4 bytes per instruction in RISC-V).
- In a von Neumann computer, instructions are stored in the same memory as all other data.
  - Allows for self-modifying code (don't do this).
- Harvard architectures store instructions in a separate (usually red-only) memory.

#### Memory transfer instructions

Note how the dynamic contents of a register influences which memory address we access.

![[Pasted image 20250105162940.png]]
