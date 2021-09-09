---
title: Create and configure file systems 
date: 2021-09-09
authors:
  - Muse Sisay
---


## Setting up partition label 

- What is the use of setting partition label?

- We can set part label using 
	- `e2label` or `tune2fs` commands
	- or set label name during filesystem setup with the `-L` option

Note : e2label and tune2fs can only create label on partiton formatted as ext2,ext3 or ext4.

- `ntfslabel` command can be used to setup label for ntfs filesystem.
- `mkswap` can be used to set up swap space label

### Set label 

```bash
e2label /dev/sdb1 LABEL
```
```console
tune2fs -L salesDocuments /dev/sdb2
```
```console
mkfs.ext4 -L backup /dev/sdb3
```
```console
mkswap -L  swapLabel DEV|FILE
```
To remove a label from a partition pass an empty string.
```console
e2label /dev/sdb5 ""
```

### Check label  

- Using `lsblk`
```console
$ lsblk --fs
...
sdb
├─sdb1          ext4        backup	        117ec561-ef19-45dd-9fd3-430c7d88ff39
├─sdb2          ext4        salesDocuments  a13c5f92-276a-400c-847d-4a86b7f5a9d6
├─sdb3          ext4        backup          c04002a7-5b10-400b-8325-fc13e5042575
└─sdb4          xfs                         116972a1-cd40-413d-ab2f-8f9e9b6aaf9b
```

- Using e2label command
```console
$ e2label /dev/sdb1
backup
```

### Mount using partition label
Replace device name with `LABEL=label` .
```text
LABEL=salesDocuments  /sales  ext4 defaults 0  0 
```
