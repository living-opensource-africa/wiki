---
title: "Search for files"
summary : Find command
date: 2021-09-06
authors:
  - Muse Sisay
---


!!! warning 
    Topic not complete.
    
    
```text
1 = w  
2 = x   
3 = wx 
4 = r   
5 = rx  
6 = rw  
7 = rwx  
```



## Find file based on Permission 

- `find . -perm 764`  will match a file with the exact permission
- `find . -perm -764` will match any files with atleast `rwx` for the user, `rw` for the group and `r` for  the world.
- `find . -perm /764`  will match a file which contains any of `rwx` for user , `rw` for group , `r` for world.

[Read More](https://askubuntu.com/questions/829716/diffrence-between-perm-mode-perm-mode-in-find-command)


### Examples

```bash
# file: find-perm.sh
# Create files with diffrent combination
# of ugo permission
#!/bin/bash
mkdir test-dir-perm

if [[ "$?" != 0  ]];then
	echo "Folder already exists"
	exit 0
fi

for uperm in {0..7};do 
  for gperm in {0..7}; do
    for operm in {0..7}; do

      touch file$uperm$gperm$operm; 
      chmod $uperm$gperm$operm file$uperm$gperm$operm;
      
    done
  done 
done
```

```bash
find . -perm 646 
```
This will do an exact match. So will only ouput the file with permission 646. i.e `file646`.


```bash
find . -perm -646 # , 'rw' for user
find . -perm -u+rw,g+w,o+rw
```
 
The least required permission for `u` is 6 so it will try to find files with  = 6 or 7  for `u`  
for `g` = 4 5 6 7  
for `o` = 6 7  

For example it will match `file656`, `file647`, `file767`


```bash
find . -perm /600
```

```bash
find . -perm /646
```
!!! bug "Need Further reading"

 