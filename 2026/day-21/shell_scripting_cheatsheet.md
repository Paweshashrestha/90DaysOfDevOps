---
# Shell Scripting Cheat Sheet (DevOps Quick Reference)

A quick reference for commonly used **Bash scripting concepts, syntax, and commands**.
---

# Quick Reference Table

| Topic    | Key Syntax               | Example                            |
| -------- | ------------------------ | ---------------------------------- |
| Variable | `VAR="value"`            | `NAME="DevOps"`                    |
| Argument | `$1`, `$2`               | `./script.sh arg1`                 |
| If       | `if [ condition ]; then` | `if [ -f file ]; then`             |
| For loop | `for i in list; do`      | `for i in 1 2 3; do`               |
| Function | `name() { ... }`         | `greet() { echo "Hi"; }`           |
| Grep     | `grep pattern file`      | `grep -i "error" log.txt`          |
| Awk      | `awk '{print $1}' file`  | `awk -F: '{print $1}' /etc/passwd` |
| Sed      | `sed 's/old/new/g' file` | `sed -i 's/foo/bar/g' file`        |

---

# 1. Basics

## Shebang

```bash
#!/bin/bash
```

Tells the system to run the script using the **Bash interpreter**.

---

## Running a Script

```bash
chmod +x script.sh
./script.sh
```

Or run directly:

```bash
bash script.sh
```

---

## Comments

Single line comment

```bash
# This is a comment
```

Inline comment

```bash
echo "Hello" # print greeting
```

---

## Variables

Declare variable

```bash
NAME="DevOps"
```

Use variable

```bash
echo $NAME
```

Quoting differences

```bash
"$VAR"   # expands variable
'$VAR'   # literal string
```

---

## Reading User Input

```bash
read -p "Enter your name: " NAME
echo "Hello $NAME"
```

---

## Command Line Arguments

```bash
$0  # script name
$1  # first argument
$2  # second argument
$#  # number of arguments
$@  # all arguments
$?  # last command exit status
```

Example

```bash
./script.sh file.txt
echo $1
```

---

# 2. Operators and Conditionals

## String Comparisons

```bash
[ "$a" = "$b" ]
[ "$a" != "$b" ]
[ -z "$a" ]   # empty string
[ -n "$a" ]   # not empty
```

---

## Integer Comparisons

```bash
[ $a -eq $b ]  # equal
[ $a -ne $b ]  # not equal
[ $a -lt $b ]  # less than
[ $a -gt $b ]  # greater than
[ $a -le $b ]  # <=
[ $a -ge $b ]  # >=
```

---

## File Test Operators

```bash
-f file   # file exists
-d dir    # directory exists
-e file   # exists
-r file   # readable
-w file   # writable
-x file   # executable
-s file   # not empty
```

Example

```bash
if [ -f "data.txt" ]; then
echo "File exists"
fi
```

---

## If / Elif / Else

```bash
if [ $a -gt 10 ]; then
echo "Greater"
elif [ $a -eq 10 ]; then
echo "Equal"
else
echo "Smaller"
fi
```

---

## Logical Operators

```bash
[ $a -gt 5 ] && echo "Yes"
[ $a -lt 5 ] || echo "No"
! [ -f file ]
```

---

## Case Statement

```bash
case $1 in
start)
echo "Starting service"
;;
stop)
echo "Stopping service"
;;
*)
echo "Unknown command"
;;
esac
```

---

# 3. Loops

## For Loop (List)

```bash
for i in 1 2 3
do
echo $i
done
```

---

## C-Style For Loop

```bash
for ((i=0;i<5;i++))
do
echo $i
done
```

---

## While Loop

```bash
count=1

while [ $count -le 5 ]
do
echo $count
((count++))
done
```

---

## Until Loop

Runs until condition becomes true.

```bash
count=1

until [ $count -gt 5 ]
do
echo $count
((count++))
done
```

---

## Loop Control

Break loop

```bash
break
```

Skip iteration

```bash
continue
```

---

## Loop Through Files

```bash
for file in *.log
do
echo $file
done
```

---

## Loop Through Command Output

```bash
cat file.txt | while read line
do
echo $line
done
```

---

# 4. Functions

## Defining a Function

```bash
greet() {
echo "Hello"
}
```

---

## Calling Function

```bash
greet
```

---

## Passing Arguments

```bash
sum() {
echo $(($1 + $2))
}

sum 5 3
```

---

## Return vs Echo

```bash
return 0
```

Returns **exit status**.

```bash
echo result
```

Returns **output value**.

---

## Local Variables

```bash
myfunc() {
local name="DevOps"
echo $name
}
```

---

# 5. Text Processing Commands

## Grep

Search text patterns.

```bash
grep "error" file
grep -i "error" file
grep -r "error" /var/log
grep -n "error" file
grep -c "error" file
grep -v "info" file
grep -E "error|failed"
```

---

## Awk

Print columns.

```bash
awk '{print $1}' file
```

Custom delimiter

```bash
awk -F: '{print $1}' /etc/passwd
```

BEGIN / END

```bash
awk 'BEGIN {print "Start"} {print $1} END {print "End"}' file
```

---

## Sed

Substitution

```bash
sed 's/foo/bar/g' file
```

Delete lines

```bash
sed '3d' file
```

Edit file in place

```bash
sed -i 's/foo/bar/g' file
```

---

## Cut

Extract columns

```bash
cut -d: -f1 /etc/passwd
```

---

## Sort

```bash
sort file
sort -n file
sort -r file
sort -u file
```

---

## Uniq

Remove duplicates

```bash
uniq file
uniq -c file
```

---

## Tr

Translate characters

```bash
tr 'a-z' 'A-Z'
```

Delete characters

```bash
tr -d '\r'
```

---

## WC

Count lines/words/characters.

```bash
wc -l file
wc -w file
wc -c file
```

---

## Head / Tail

First lines

```bash
head -10 file
```

Last lines

```bash
tail -10 file
```

Live log monitoring

```bash
tail -f logfile
```

---

# 6. Useful One-Liners

Delete files older than 7 days

```bash
find /path -name "*.log" -mtime +7 -delete
```

---

Count lines in all log files

```bash
wc -l *.log
```

---

Replace string across multiple files

```bash
sed -i 's/old/new/g' *.txt
```

---

Check if service is running

```bash
systemctl status nginx
```

---

Monitor disk usage

```bash
df -h
```

Alert if disk > 80%

```bash
df -h | awk '$5+0 > 80 {print "Disk Full:", $0}'
```

---

Real-time error monitoring

```bash
tail -f /var/log/syslog | grep ERROR
```

---

# 7. Error Handling and Debugging

## Exit Codes

```bash
exit 0   # success
exit 1   # error
```

Check exit status

```bash
echo $?
```

---

## Exit on Error

```bash
set -e
```

Script stops if any command fails.

---

## Unset Variables Error

```bash
set -u
```

Error if variable is undefined.

---

## Pipe Failure Detection

```bash
set -o pipefail
```

Captures errors in pipelines.

---

## Debug Mode

```bash
set -x
```

Shows commands while executing.

---

## Trap

Run cleanup on exit.

```bash
trap 'echo "Cleaning up"' EXIT
```

Example

```bash
trap 'rm temp.txt' EXIT
```

---

# Key Takeaways

1. Bash scripting helps **automate repetitive system tasks**.
2. Linux tools like **grep, awk, sed, and sort are powerful for log processing**.
3. Good scripts include **validation, error handling, and logging**.

---
