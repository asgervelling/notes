The New Technology File System [[File Systems|file system]].
Includes a boot sector, a master file table, a file storage area and a copy of the master file table.
Master file table is a list of the files stored on the disk called the SMFT. Those files are stored in the file storage area (MFT stands for Master File Table).
When deleting a file, you are simply removing the pointer from the MFT to whatever address in the file storage area that it was pointing to. You are not actually deleting a file.
If you are a bad guy it is better to rename, overwrite and then delete your file.

##### Formatting a Hard Disk
Formatting a hard disk creates a new MFT. All existing files are still available, but their addresses are unknown.