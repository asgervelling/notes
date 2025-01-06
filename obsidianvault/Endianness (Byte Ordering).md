- Big endian: Most significant byte has lowest address ("comes first").
  - SPARC, POWER, Internet protocols.
- Little endian: Most significant byte has highest address ("comes last").
  - x86, ARM (mostly), [[RISC-V]].

## Byte Ordering Example

- Variable has 4-byte value of `0x01234567`.
- Address `&x` is `0x100`.
  - No matter what, the address of an object is always the address of the first byte in the object (counting from lowest addresses).

![[Pasted image 20250105162412.png]]

## Important Note

The difference is not visible unless you start decomposing integers as bytes with memory operations. Bit-shifting etc. always acts as expected.


## Exercises
### COD 2.5
*Show how the value `0xabcdef12` would be arranged in memory of a little-endian and a big-endian machine. Assume the data are stored starting at address 0 and that the word size is 4 bytes.*

Big endian:    `ab` `cd` `ef` `12`
Little endian: `12` `ef` `cd` `ab`
