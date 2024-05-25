# simple-file-system
A simple file system for learning purpose.
As of now , build a simple file system on top of the disk interface librray built seperately. This file system should provide efficient storage. In particular, access to file blocks should be efficient and the amount of metadata needed to implement the file system should be small.

This assignment revolves around three of the functions provided by a file system:

Naming. Human-readable file names must be mapped to the disk blocks that they are associated with.
Organization. File systems (typically) have mechanisms humans can use to organize information. Computer scientists like hierarchies, as a result, the tree directory structure has become the dominant mechanism for organization.
Persistence. Data is entrusted to the file system and is expected to remain there (uncorrupted and undeleted) until an explicit request is made to delete it. This aspect includes support for allocation and deletion of files; as syntactic sugar, file systems also provide mechanisms to modify existing files.
Important issues that we will treat lightly include protection (ensuring only authorized programs can read/write/delete/create files) and performance (disks are slow: most research on file systems concentrates solely on performance).
To make the assignment concrete, we discuss a common file system organization below: you are encouraged to construct your own ideas, and to simply use this discussion to understand the issues involved.

There are three main data structures file systems manage:

```A disk block freelist. ```

This is a simple data structure (for instance, a bitmap) that tracks what blocks on the disk have been allocated. This data structure is, for obvious reasons, persistent (i.e., on disk). You will have to establish some convention for locating it across reboots.

```Inodes.```

An inode is persistent memory that contains pointers to disk blocks or to ``indirect blocks'' (you can think of these as other inodes). Inodes are used to map a file to the disk representation of a file: each file has its own inode, the inode has pointers to all disk blocks that make up that file.

```Directories.```
Directories are persistent mappings of names to inode numbers for all the files and directories that the directory is responsible for.

```The actions to create file:```

Allocate and initialize an inode.
Write the mapping between the inode and file name in the file's directory (usually the current working directory).
Write this information to disk.

```To grow a file:```

Load the file's associated inode into memory.
Allocate disk blocks (mark them as allocated in your freelist).
Modify the file's inode to point to these blocks.
Write the data the user gives to these blocks.
Flush all modifications to disk.


```To shrink a file:```

Load the file's associated inode into memory.
Remove pointers to the disk blocks from the inode.
Mark the disk blocks as free.
Flush all modifications to disk.


Modifying directories is similar.

Be careful about when modifications to file system state are made persistent. For instance, in the case of creating a file, if we flush modifications to disk after stage (1), a crash that happened before (2) would cause us to have a disk block allocated that was not used. If we did not flush modifications to disk at all, then a crash would cause the file to be lost. You will have to pay particular attention to this and similar problems in the third part of the assignment.

Survive random crashrs -- maintain state persistently. In particular, it should be able to run, be powered down, and then be ``rebooted'' in the state that it was in when it was shutdown (i.e., all files that were written at shutdown time should still be there, and no garbage files should suddenly appear).

In addition to correctness, consider space efficiency: file system should have a low overhead per byte of file data.

```TODO :```
### Implement block caching.

