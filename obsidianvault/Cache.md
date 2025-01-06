## Definition
A smaller, faster storage device that acts as a staging area for a subset of the data in a larger, slower device.

- **Fundamental idea of the memory hierarchy**
  The smaller and faster device at level *k* acts as a cache for the larger slower device at level *k + 1*.
- **Why do they work?**
  Because of locality, most accesses tend to be towards the top of the hierarchy.
- **The ideal**
  A huge pool of storage that is as cheap as at the bottom of the hierarchy, but as fast as at the top of the hierarchy.

## Visual Examples

There are visual examples of caches in [[Cache Basics - Visual Examples]].

## Types of Cache Misses

- **Cold/compulsory miss**
  - Occur when the cache is empty
  - Unavoidable when a program first starts.
- **Conflict miss**
  - Most caches limit blocks at level *k + 1* to a small subset of the slots at level *k*.
   -  Example: Block *i* can only be located in slot *i* mod 4.
  - Causes conflicts when cache is large enough, but the blocks being accessed all map to the same slot.
- **Capacity miss**
  - Occurs when program working set exceeds size of cache.

## Cache Organization and Operation

## Terms
The following are some basic cache terms that you should be comfortable
with.

**Cache block**
- The basic unit for cache storage. May contain multiple bytes/words of data.
**Cache line**
- Same as cache block. Note that this is not the same thing as a “row” of cache.
**Cache set**
- A “row” in the cache. The number of blocks per set is determined by the layout of the cache (e.g. direct mapped, set-associative, or fully associative).
**Tag**
- A unique identifier for a group of data. Because different regions of memory may be mapped into a block, the tag is used to differentiate between them.
**valid bit**
- A bit of information that indicates whether the data in a block is valid (1) or not (0).

## Locating Data in the Cache

Given an address, we can determine whether the data at that memory location is in the cache. To do so, we use the following procedure:

1.  Use the set index to determine which cache set the address should reside in.
2. For each block in the corresponding cache set, compare the tag associated with that block to the tag from the memory address. If there is a match, proceed to the next step. Otherwise, the data is not in the cache.
3. For the block where the data was found, look at the valid bit. If it is 1, the data is in the cache, otherwise it is not.

![[Pasted image 20250102121358.png]]


All of the information needed to locate the data in the cache is given in
the address. Fig. 1 shows which parts of the address are used for locating
data in the cache. The least significant bits are used to determine the block
offset. If the block size is $B$ then $b = log_2(B)$ bits will be needed in the
address to specify the block offset. The next highest group of bits is the set
index and is used to determine which cache set we will look at. If S is the
number of sets in our cache, then the set index has $s = log_2(S)$ bits. Note
that in a fully-associative cache, there is only 1 set so the set index will not
exist. The remaining bits are used for the tag. If $l$ is the length of the
address (in bits), then the number of tag bits is $t = l − b − s$.

### Loading Data into the Cache

If the requested data is not in the cache, it will need to be brought in from memory. 
Along with the requested address, we will also bring in other addresses that are near it, to take advantage of spatial locality.
To determine which addresses will also be brought in, we find the starting and ending address of the range that will be brought in.
- The starting address can be found by zeroing out the block offset part of the address.
- For the ending address, we replace the block offset with all 1's.

Note that the size of this range will always be the size of a cache block. The data in tha range will be brought in and placed in one of the blocks in the cache.

Depending on the cache organization, there may be multiple places to put data.
- In a [[Direct Mapped Cache|direct-mapped cache]], there is only one block in the cache where the data can go.
- In [[Set-Associative Cache|set-associative]] and [[Fully-Associative Cache|fully-associative]] caches, we must choose among $n$ different blocks.
  - There are $n$ locations in each set for an n-way set-associative cache, while an incoming block of data can be placed in any location in a fully-associative cache.

If none of the possible locations are free (i.e. none of the blocks in the cache set have a valid bit set to 0) then we must evict some of the old data. There are various ways to decide which blocks of data will be replaced, but the most popular is the [[Caching - Eviction Policies#Least Recently Used|least recently used (LRU)]] scheme ([[Caching - Eviction Policies|eviction policy]]). If using an LRU scheme, then each block in the cache must have LRU bits to track which block is the oldest.




### General structure of a cache for S, L and B

![[Pasted image 20250102120744.png]]

![[Pasted image 20250102134935.png]]
![[Pasted image 20250102135027.png]]


### Byte vs Word Addressing in Caches

A lot of confusion can arise when talking about byte vs word addressing and its impact on caches. Most modern architectures (MIPS included) use word addressing. For 32-bit [[Instruction Set Architecture|ISAs]], registers have a size of 32 bits (a word). It doesn't make much sense to specify which bytes you are going to use if you are going to place them in a word sized register.
As we have seen in the Patterson & Hennessy textbook,
addresses are shifted left by 2 bits to convert them from a word to a byte
address. This translation from words to bytes is hidden from the ISA so
if we specify a load from memory address X, we can’t tell that it is really
being treated as $4*X$ in the processor.
When dealing with caches, we have seen that the memory address is
split into three parts: block offset, set index, and tag. These three parts are
then used to determine whether the address requested is in the cache. The
question is, if we are given a word address do we first have to “shift left” by
2 by adding “00” to the end of the address? Adding 2 bits to the end would
affect which bits of the original address (i.e. the one without the 00 at the
end) are used for the offset, index, and tag. As we will see, this leads to
undesirable behavior in the cache, which will become apparent with a small
example.
Suppose that your cache has a block size of 4 words. This means that
the block offset is the 2 LSBs of your address. If we were to add “00” to the
end of every address then the block offset would always be “00.” This would
mean that we could never access any word other than the one at position
00 in our block.
Always be aware of whether the architecture you are being asked about
has word or byte addressing. We will, unless otherwise specified, be assuming
a MIPS-like architecture. This means that you will generally be working
with word addressing. Make sure that all your parameters (e.g. block size)
have the same unit (either word or byte) as the type of addressing you are
using.

## Exercises

### COD 5.1
*Assume memory is byte addressable and words are 32 bits, unless specified otherwise.*

**5.1.** *In this exercise, we look at memory locality properties of matrix computation. The following code is written in C, where elements within the same row are stored contiguously. Assume each word is a 32-bit integer.*

```c
for (I=0; I<8; I++)
  for (J=0; J<8000; J++)
    A[I][J] = B[I][0] + A[J][I]
```

**5.1.1.** *How many 32-bit integers can be stored in a 16-byte cache block?*
4, because $\frac{16}{\frac{32}{8}} = \frac{16}{4} = 4$.

**5.1.2.** *Which variable references exhibit temporal [[Memory#Locality of Reference|locality]]?*
$I$ and $J$.

**5.1.3.** *Which variable references exhibit spatial locality?*
`A[I][J]`.

*Note for 5.1.4 - 5.1.6:* Matlab uses Column-major order.

*Locality is affected by both the reference order and data layout. The same computation can also be written below in Matlab, which differs from C in that it stores matrix elements within the same column contiguously in memory:*

```matlab
for I=1:8
  for J=1:8000
    A(I,J) = B(I,0) + A(J,I);
  end
end
```

**5.1.4.** *Which variable references exhibit temporal locality?*
I and J.

**5.1.5.** *Which variable references exhibit spatial locality?*
`A(J, I)`.

**5.1.6.** *How many 16-byte cache blocks are needed to store all 32-bit matrix elements being referenced using Matlab's matrix storage? How many using C's matrix storage? (Assume each row contains more than one element.)*
Number of elements, $n = 8 \cdot 8000 = 64000$.
32 bits = 4 bytes.
How many 16-byte (128-bit) cache blocks are needed to hold $64000 * 4 = 256000$ bytes?
$256000 / 16 = 16000$.
