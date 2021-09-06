---
title: 'List, set, and change standard file permissions'
date: 2021-09-06
authors:
  - Muse Sisay
---

!!! warning 
    Topic not complete.
    
```text
421
rwx
```

## Effect `rwx` on Files and Directories

| PERMISSION | EFFECT ON FILES                      | EFFECT ON DIRECTORIES                                                                                                                                                      |
| :--------: | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     r      | Contents of the file can be read.    | Contents of the directory (the file names) can be listed.                                                                                                                  |
|     w      | Contents of the file can be changed. | Any file in the directory can be created or deleted.                                                                                                                       |
|     x      | Files can be executed as commands.   | Contents of the directory can be accessed. (You can change into the directory, read information about its files, and access its files if the files' permissions allow it.) |

```bash
# file: test-dir-perm.sh
# Create folders with diffrent combination
# for user perm
#!/bin/bash
mkdir test-dir-perm

if [[ "$?" != 0  ]];then
	echo "Folder already exists"
	exit 0
fi

cd test-dir-perm

for uperm in {0..7} ; do

	# mkdir -m ${uperm}55 dir-${uperm}55

	mkdir dir-${uperm}55
	# create sample file
	echo "Hello"  > dir-${uperm}55/file.txt
	chmod ${uperm}55  dir-${uperm}55

done
```

| DIRECTORY | PERM | cd  | ls   | touch |  cat  | rm  |
| --------- | ---- | --- | ---- | :---: | :---: | --- |
| ./dir-055 | .    | no  | no   |  no   |  no   | no  |
| ./dir-155 | --X  | yes | no   |  no   |  yes  | no  |
| ./dir-255 | -W-  | no  | no   |  no   |  no   | no  |
| ./dir-355 | -XW  | yes | no   |  yes  |  yes  | yes |
| ./dir-455 | R--  | no  | yes* |  no   |  no*  | no  |
| ./dir-555 | R-X  | yes | yes  |  no   |  yes  | no  |
| ./dir-655 | RW-  | no  | yes* |  no   |  no   | no  |
| ./dir-755 | RWX  | yes | yes  |  yes  |  yes  | yes |

[Unix File Permissions by @Perderabo](https://www.unix.com/tips-and-tutorials/19060-unix-file-permissions.html) Please take the time and try to understand it. 

??? cite
    On a directory, that x is officially called `search permission`. 
    You need `x` to use a directory in a pathname. So if you try
    "cat /etc/passwd", you will need `x` on `/ `and `/etc`. You also need 
    `x` to cd into a directory. Suppose you have read but not search (x)
    permission on a directory. What can you do? Not much. You can use `ls` 
    to view the file names. Even "ls -l" will not work. Read access without 
    search permission is not very useful. Still that is better than having 
    only write permission on a directory...**that is completely useless**. I 
    have not seen any other documentation that states this explicitly, so 
    let me repeat it: write but no execute permission on a directory grants 
    nothing at all.Suppose you have search (x) permission but no read permission
    on a directory. Now you can open files in the directory if you happen to know
    the file's name. You can cd into the directory. And that is it. You cannot even
    create a new file. Adding write permission will allow you to create files. And you
    can then delete files if you happen to know their name.


What that means is that 

- with `read` permission you are able to only view filenames
- with  `execute` permission you are able to open files within the directory , if you happen to know the file name and have sufficient permission on the file.
- `write` permission alone is useless.( pointless to have it)




## Effects of Special Permissions on Files and Directories

```text
suid =  4
sgid = 2
sticky-bit = 1
```

| SPECIAL PERMISSION | EFFECT ON FILES                                                               | EFFECT ON DIRECTORIES                                                                                                                          |
| :----------------: | ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
|     u+s (suid)     | File executes as the user that owns the file, not the user that ran the file. | No effect.                                                                                                                                     |
|     g+s (sgid)     | File executes as the group that owns the file.                                | Files newly created in the directory have their group owner set to match the group owner of the directory.                                     |
|    o+t (sticky)    | No effect.                                                                    | Users with write access to the directory can only remove files that they own; they cannot remove or force saves to files owned by other users. |


## File Access control list (FACL) 

- a second level of discretionary access control
- Usually `.` symbol at the end of the permission block indicates ACL is supported
- We use `setfacl` set file access control and `getfacl` list file access control
- `skipped` Default ACL's & Removing ACl entries


:: I think I have to mention Redhat or something. I copied the tables 
from their book.