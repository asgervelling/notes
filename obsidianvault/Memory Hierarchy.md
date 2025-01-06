### Some fundamental and enduring properties of hardware and software
- Fast storage is expensive, has smaller capacity, and requires more power.
- There is a large gap between computational speed and memory speed.
- Well-written programs tend to exhibit good locality.

These properties suggest an approach for organising memory and other storage
systems known as a memory hierarchy.

![[Pasted image 20250102115411.png]]

## Caches

### Definition of *cache*
A smaller, faster storage device that acts as a staging area for a subset of the data in a larger, slower device.

- **Fundamental idea of the memory hierarchy**
  The smaller and faster device at level *k* acts as a cache for the larger slower device at level *k + 1*.
- **Why do they work?**
  Because of locality, most accesses tend to be towards the top of the hierarchy.
- **The ideal**
  A huge pool of storage that is as cheap as at the bottom of the hierarchy, but as fast as at the top of the hierarchy.

For more information, see [[Cache]].