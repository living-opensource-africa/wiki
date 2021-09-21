---
title:  Configure and manage swap space
# summary: 
date: 2021-09-21
authors:
  - Muse Sisay
---

- A swap space could be a file or a partition (or  both).
    - incase of  a file it needs to be allocated using  fallocate or dd command.
    - count size for a size of 1G is 1024x1024 = 1048576.
    - file perm should be 600

```
$ sudo fallocate -l 1G /swapfile1
```
The command will allocate swapfile in `/` with 1Gb.
```
$ sudo dd if=/dev/zero of=/swapfile2 bs=1024  count=1048576
$ sudo dd if=/dev/zero of=/swapfile3 bs=1G count=1 
```
shows diffrent ways of specifying size in dd command. 

-`bs` refers to the block size. `M` in binary units (eg MB). suffix `B` and size would interms of decimal units.    
-`count` just count
   
 ### Steps in creating swap space
1. Create the swap space
    1. *Swap File* create a file using fallocate or dd command
    2. *Swap partition* create a partition with linux swap  type
2. issue mkswap on  file/partition
3. turn swapon for file or partition

 ### Summary 

- List all active swap space ( swapon --show, free, top, htop commands)
- Enable or disable swap space; individual or all swap space ( swapon an swapoff commands)
- For persistence add an entry for a the swap partition in /etc/fstab


* Note
- You might get the following error when using fallocate to allocate swao space `swapon: /swapfile: swapon failed: Invalid argument`. 
```
 Files with holes
       The swap file implementation in the kernel expects to be able to write  to  the  file  directly,  without  the
       assistance  of the filesystem.  This is a problem on files with holes or on copy-on-write files on filesystems
       like Btrfs.

       Commands like cp(1) or truncate(1) create files with holes.  These files will be rejected by swapon.

       Preallocated files created by fallocate(1) may be interpreted  as  files  with  holes  too  depending  of  the
       filesystem.  Preallocated swap files are supported on XFS since Linux 4.18.

       The most portable solution to create a swap file is to use dd(1) and /dev/zero.
```
