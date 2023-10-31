Situation: *We've found a hard disk and are looking for a specific file.*

Questions we're trying to answer:
	1. Was the file downloaded to the computer?
	   Look at its creation timestamp. If it was downloaded, that information would not be present.
	2. Was the file copied from another medium?

- We make a copy of the hard disk. 
   When copying a hard-disk, do check that the copy and the original are the same by checking both their hashes, using a hashing algorithm with no collisions, such as one of the SHA algorithms.

- We list all the files on the copy.
  We look at the different partitions of the hard disk. By analyzing its master boot record (MBR), we can find information that shows what file system the hard disk uses. The MBR also shows us whether the hard disk is bootable, i.e. it has an operating system.
  E.g. if the [[File Systems|file system]] is NTFS, we can start by listing the files on the SMFT (list of files).
  When we have the files, we can start looking at when they were created, accessed or modified, provided the disk was not partitioned, resetting its master file table.

**Book recommendation**: Brian Carrier - 'File System Forensic Analysis'
