---
title: 'Create and manage RAID devices'
date: 2021-09-06
authors:
  - Muse Sisay
---

# RAID
Redundant Array of Independent Disks

- A glance at what RAID is. /RAID is not a backup

### RAID Levels

| LEVEL              |    0     |     1     |        5        |       6       |
| ------------------ | :------: | :-------: | :-------------: | :-----------: |
| .                  | striping | mirroring | parity striping | double parity |
| Min. disk required |    2     |     2     |        3        |       4       |

- **RAID 5** supports a single disk failure.
- **RAID 6** supports upto 2 disk failures.



```shell
yum install mdadm
```

General structure of `mdadm` command,
```text
mdadm [mode] <raiddevice> [options] <component-devices>
```
if no mode is specified it will default to `--manage`. 

!!! note
    RAID device comes before component devices.

## Managing RAID Array

!!! info
    I will be referring to the RAID array as `/dev/md0`. All commands are run as root user


### Create

- Create `level 0` RAID with 2 active devices and 1 spare
```console
mdadm --create /dev/md0 --level=0 --raid-devices=2 \
      /dev/sdb1 /dev/sdb2 --spare=1 /dev/sdb3
```
**Note** We explicitly specify the number of disks that are going to be used in our array

- Create `level 5` RAID with 3 active devices
```console
mdadm --create /dev/md0 --level=5 --raid-devices=3 \
      /dev/sdb1 /dev/sdb2 /dev/sdb3
```

- Finally create a filesystem 
```
mkfs.ext4 /dev/md0
```

### Check Status 
```console
mdadm --detail /dev/md0
cat /proc/mdstat
```

### Automounting RAID array at boot
- First save the configuration to `/etc/mdadm.conf`
```console
mdadm --detail --scan >> /etc/mdadm.conf
```
- Then add an entry in `/etc/fstab`
```text
# RAID
/dev/md0    /mnt/shared   ext4  defaults 0  0
```

- To read *array info* from mdadm.conf
```console
mdadm --assemble --scan
```

### Removing RAID array

- Make sure the drive is unmounted, then 
```console
umount /dev/md0
mdam --stop /dev/md0
```
Then remove entries for the array from `/etc/mdadm.conf` and `/etc/fstab`. There are other steps involved, but that is out of the scope of the LFCS exam.

### More operations

- Mark a disk faulty
```console
madam /dev/md0 --fail /dev/sdb1
```

- Remove disk from array ( should not be active)
```console
mdadm /dev/md0 --remove /dev/sdb1
```
- Add spare to array
```console
mdadm /dev/md0 --add /dev/sdb2
```
- Grow RAID ARRAY:
```console
mdadm --add /dev/md0 /dev/sdb2
mdadm --grow /dev/md0 --raid-devices=4  
```


## Shortcut discussed 

Vim shortcut we discussed : 

- `: set nu` or `: set number` to show line number 
- `LINE + G` or `: LINE` to jump to LINE
- `$` to move to end of line
- `gg` to move to the end of a file
- `G` to move to the start of a file