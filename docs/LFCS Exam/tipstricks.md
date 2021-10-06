---
title: Tips and Tricks 
date: 2021-09-07
authors:
  - Muse Sisay
---

## Terminal Tricks

- `cd -` move(return) back to previous directory
- ++ctrl+l++ clear screen

### Moving around
- ++ctrl+a++ to jump to the begining of a line
- ++ctrl+e++ to jump to the end of the line
- ++ctrl+u++ remove everything on the left-hand side of the cursor 
- ++ctrl+k++ remove everything on the right-hand side of the cursor 
- ++ctrl+left++ or ++ctrl+right++

## Vim tricks

!!! note
    If you are new to vim run **vimtutor** to get a crash course on vim.

- `: set nu` or `: set number` to show line number 
- ++"LINE"+shift+"g"++ or `: LINE` to jump to LINE
- ++"$"++ to move to end of line
- ++"g"+"g"++ to move to the end of a file
- ++shift+"g"++ to move to the start of a file
- ++"D"+"G"++ remove all lines

## Useful Commands 

- **rpmquery** to check if a package is installed.
```console
$ rpmquery [package]
```
Alternatively you can use `yum list` to find out if a package is installed. The `@` symbol before the repository name indicates if the package is installed.
```console 
$ sudo yum list | autofs
libsss_autofs.x86_64                                   2.4.0-9.el8                                         @anaconda
autofs.x86_64                                          1:5.1.4-48.el8                                      BaseOS
```
- **nfsstat** show currently nfs mounted mounts
```console
$ nfsstat --mounts
```

## Snippets 

- Disbaling all enabled reposiotries. 
```console
$ sed -i 's/enabled=1/enabled=0/' /etc/yum.repos.d/*
$ yum clean all
```

Before setting up local repositories it is a good idea to disable default repo's.

- Setting password from STDIN
```console
$ echo "password" | passwd $USER --stdin
```

- Input Redirection. 
```console
$ cat > mongodb_cgroup_memory.te <<EOF
module mongodb_cgroup_memory 1.0;
require {
      type cgroup_t;
      type mongod_t;
      class dir search;
      class file { getattr open read };
}
#============= mongod_t ==============
allow mongod_t cgroup_t:dir search;
allow mongod_t cgroup_t:file { getattr open read };
EOF
```
