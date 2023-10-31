Great tool: Volatility (2007).

Types of acquisition (getting the data) in memory forensics:
- Live acquisition (taking a copy of turned on memory)
- Live analysis
- Dead acquisition
- (1 more from slides Oct. 13 2023)

In RAM, we can find
- Running processes
- Finished processes
- Network connections
- Memory-only malware
- Encryption keys

##### Phases in Memory Forensics Analysis
1. Find irregular processes
   Could be one that's trying to hide, or one that has strange parents.
   Could be one that doesn't normally communicate over the internet (like explorer.exe).
   One that has many handles or contains code injection.
1. Analyze the processes' DLL files
2. Look at network artifacts
3. Look for code injection
4. Dump suspicious processes and analyze them


**Book Recommendation**: 'The Art of Memory Forensics'