# Day 10 Challenge

## Files Created

devops.txt
notes.txt
script.sh

## Permission Changes

script.sh
before: rw-r--r--
after: rwxr-xr-x

devops.txt
before: rw-r--r--
after: r--r--r--

notes.txt
set to 640

project/
set to 755

## Commands Used

touch devops.txt
echo "Learning DevOps is fun" > notes.txt
vim script.sh

cat notes.txt
vim -R script.sh

head -n 5 /etc/passwd
tail -n 5 /etc/passwd

chmod +x script.sh
chmod -w devops.txt
chmod 640 notes.txt

mkdir project
chmod 755 project

## What I Learned

1. Linux permissions control file security
2. chmod modifies read write execute permissions
3. directories require execute permission to enter

```

---
```
