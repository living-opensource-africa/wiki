---
title: Setting Disk quota
date: 2021-11-09
authors:
  - Muse Sisay
---
- disk quota managed through the quota* command. Make sure to have it installed it and that the kernel supports this feature.
- remount the partition using `usrquota,grpquota` option. Edit /etc/fstab to mount partition using the following option during boot.

## Steps in defining disk quota

1. Enable(add) quota option when mounting. Edit /etc/fstab for persistence. Use the following mount options
```console
- For user quota : `usrquota`, `uquota`
- For group quota : `grpquota`, `gquota`
- For project quota : `pquota`. (before using this option make sure the file system you are using supports project quota)
```

2. Create a quota file. It contains quota usage information for a specific file system.
```console
$ quotacheck -cugmva
```
`-c`  Don't  read  existing  quota files. Just perform a new scan and save it to disk. This option will a quota file for keeping track of usage.  
`-m` Don't try to remount file system read-only  
`-u`,`-g` option indicate user and group quota should be created.  
  Alternatively you can create the quota file for the  partition you want instead of creating for all the partitions in fstab mounted that have quota option.
  ```console
  quotacheck -cum /dev/sda1
  ```

1. Switch quota on using `quotaon` command on the partition.
```console
quotaon /dev/sdad1
quotaon -p /dev/sda1 # check status 
```

4. Edit userquota using `edquota` command
```
$ edquota -u tux
```
to edit grace period
```
$ edquota -t
```
---
* User quota can be modified or changed using `setquota` command
```bash
$ setquota -u tux 70M  80M 0 0 /dev/sdb1
```

## Setting quota on xfs file system

- xfs has better quota management tools. `xfs_quota` is the command
- mount options are similar to ext file system. For user quota we can either user `usrquota` or `uquota`, for group quota we can either use `grpquota` or `gquota`. We also have options like `uqnoenforce` and `gqnoenforce`. `pquota`.
- There are two modes,both can run in interactive mode
    - Simple mode 
    - Expert mode

- To view quota 
```
$ xfs_quota -xc "report" DEV
```

- To set block quota limit of (30M,35M) for user `tux` on /dev/sdb4
```
$ xfs_quota -xc "limit -u bsoft=30M bhard=35M tux" /dev/sdb4 
```
- no need to run quotacheck on the disk when using xfs file system.

## Checking  quota (allocation)

```console
repquota -uv /dev/sda1
```

## Summary 

- Run command (quotacheck, setquota, repquota) without arguments to get help.
- Generate a general quota report on the system using `repquota -av` command
- edit user and group disk quota
- Display/check a user's quota 
- `requota` by default ignores user without usage
- The `quotacheck` command has no effect on XFS file systems.

```bash
$ quota -u tux 
```
