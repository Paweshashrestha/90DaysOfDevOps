---

# Day 16 – Shell Scripting Basics

Shell scripting is one of the **most important DevOps skills**.

DevOps engineers automate things like:

* server setup
* deployments
* backups
* monitoring
* log analysis
* container management

Instead of running commands manually:

```bash
apt update
apt install nginx
systemctl start nginx
```

You automate it with a script.

Example:

```bash
#!/bin/bash
apt update
apt install -y nginx
systemctl start nginx
```

Now one command runs everything.

---

# Task 1 – Your First Script

## Step 1: Create the file

```bash
touch hello.sh
```

---

## Step 2: Add shebang

Open file:

```bash
nano hello.sh
```

Add:

```bash
#!/bin/bash

echo "Hello, DevOps!"
```

---

## What is Shebang?

```
#!/bin/bash
```

This tells Linux:

> "Run this script using the **Bash interpreter**"

Breakdown:

| Part      | Meaning                  |
| --------- | ------------------------ |
| #!        | special marker           |
| /bin/bash | path to bash interpreter |

So Linux executes:

```
/bin/bash hello.sh
```

---

## Step 3: Make executable

Scripts must be executable.

```bash
chmod +x hello.sh
```

Verify:

```bash
ls -l hello.sh
```

Example output:

```
-rwxr-xr-x 1 user user 23 Mar 10 hello.sh
```

`x` = executable.

---

## Step 4: Run script

```bash
./hello.sh
```

Output:

```
Hello, DevOps!
```

---

## What Happens Without Shebang?

If you remove:

```
#!/bin/bash
```

and run:

```bash
./hello.sh
```

Possible results:

- it may fail
- or run using default shell

Example error:

```
command not found
```

Why?

Because Linux doesn't know **which interpreter should execute the script**.

You would have to run manually:

```bash
bash hello.sh
```

---

# Task 2 – Variables

Variables store values.

Example:

```bash
NAME="Pawesha"
ROLE="DevOps Engineer"
```

---

## Create Script

```
variables.sh
```

Code:

```bash
#!/bin/bash

NAME="Pawesha"
ROLE="DevOps Engineer"

echo "Hello, I am $NAME and I am a $ROLE"
```

Run:

```bash
chmod +x variables.sh
./variables.sh
```

Output:

```
Hello, I am Pawesha and I am a DevOps Engineer
```

---

## Variable Rules

Correct:

```
NAME="John"
```

Wrong:

```
NAME = "John"
```

Spaces break the variable assignment.

---

# Single Quotes vs Double Quotes

This is **VERY important**.

Example:

```
NAME="Pawesha"
```

### Double Quotes

```
echo "Hello $NAME"
```

Output:

```
Hello Pawesha
```

Variables expand.

---

### Single Quotes

```
echo 'Hello $NAME'
```

Output:

```
Hello $NAME
```

Variables **do NOT expand**.

---

## Summary

| Quotes | Behavior         |
| ------ | ---------------- |
| " "    | variables expand |
| ' '    | literal text     |

---

# Task 3 – User Input with read

Shell scripts can interact with users.

---

Create:

```
greet.sh
```

Code:

```bash
#!/bin/bash

read -p "Enter your name: " NAME
read -p "Enter your favourite tool: " TOOL

echo "Hello $NAME, your favourite tool is $TOOL"
```

Run:

```
./greet.sh
```

Example output:

```
Enter your name: Pawesha
Enter your favourite tool: Docker
Hello Pawesha, your favourite tool is Docker
```

---

## How `read` Works

```
read VARIABLE
```

Example:

```
read NAME
```

Stores input into variable.

---

## `-p` Option

```
read -p "Enter name: " NAME
```

`-p` shows prompt text.

---

# Task 4 – If Else Conditions

Conditions allow scripts to **make decisions**.

Structure:

```
if [ condition ]
then
   command
elif [ condition ]
then
   command
else
   command
fi
```

`fi` closes the if block.

---

# Script 1 – Check Number

Create:

```
check_number.sh
```

Code:

```bash
#!/bin/bash

read -p "Enter a number: " NUM

if [ $NUM -gt 0 ]
then
    echo "Number is positive"

elif [ $NUM -lt 0 ]
then
    echo "Number is negative"

else
    echo "Number is zero"
fi
```

---

## Condition Operators

| Operator | Meaning          |
| -------- | ---------------- |
| -gt      | greater than     |
| -lt      | less than        |
| -eq      | equal            |
| -ge      | greater or equal |
| -le      | less or equal    |

Example:

```
5 -gt 3
```

True.

---

# Script 2 – File Check

Create:

```
file_check.sh
```

Code:

```bash
#!/bin/bash

read -p "Enter filename: " FILE

if [ -f "$FILE" ]
then
    echo "File exists"
else
    echo "File does not exist"
fi
```

---

## File Check Flags

| Flag | Meaning          |
| ---- | ---------------- |
| -f   | file exists      |
| -d   | directory exists |
| -r   | readable         |
| -w   | writable         |
| -x   | executable       |

---

# Task 5 – Combine Everything

Create:

```
server_check.sh
```

Code:

```bash
#!/bin/bash

SERVICE="nginx"

read -p "Do you want to check the status of $SERVICE? (y/n): " ANSWER

if [ "$ANSWER" = "y" ]
then
    STATUS=$(systemctl is-active $SERVICE)

    if [ "$STATUS" = "active" ]
    then
        echo "$SERVICE is running"
    else
        echo "$SERVICE is not running"
    fi

else
    echo "Skipped."
fi
```

---

## Explanation

### Variable

```
SERVICE="nginx"
```

Stores service name.

---

### Command Substitution

```
STATUS=$(systemctl is-active nginx)
```

Stores command output in variable.

Example:

```
active
```

---

### Nested If

Script checks:

1️⃣ user input
2️⃣ service status

---

Example run:

```
Do you want to check the status of nginx? (y/n): y
nginx is running
```

---

# Real DevOps Example Scripts

These are extremely common.

---

## Check Disk Space

```bash
#!/bin/bash

THRESHOLD=80

USAGE=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ $USAGE -gt $THRESHOLD ]
then
   echo "Disk usage critical!"
else
   echo "Disk usage normal"
fi
```

---

## Check Server Reachability

```bash
#!/bin/bash

ping -c 1 google.com > /dev/null

if [ $? -eq 0 ]
then
   echo "Internet reachable"
else
   echo "Network issue"
fi
```

`$?` = exit status.

---

# Key Shell Concepts DevOps Must Know

### `$VARIABLE`

Access variable value.

Example:

```
echo $NAME
```

---

### `$?`

Exit status of previous command.

```
0 = success
non-zero = error
```

---

### `$(command)`

Store command output.

```
DATE=$(date)
```

---

### `&&`

Run next command if success.

```
mkdir test && cd test
```

---

### `||`

Run next command if failure.

```
mkdir test || echo "Failed"
```

---

# Real DevOps Automation Example

Deploy script:

```bash
#!/bin/bash

git pull origin main
docker build -t myapp .
docker stop myapp
docker run -d -p 80:3000 myapp
```

One script deploys entire application.

---

# What I Learned (3 Key Points)

1. **Shebang defines which interpreter runs the script.**

2. **Variables and read allow scripts to store and use dynamic data.**

3. **If-else conditions allow automation decisions like checking files, services, or numbers.**

---

# Common DevOps Interview Questions

### What is shebang?

The **shebang (`#!/bin/bash`) specifies the interpreter used to execute a script.**

---

### Difference between `$()` and backticks?

Both run commands.

Modern preferred:

```
$(command)
```

Older:

```
`command`
```

---

### How do you check if a file exists in bash?

```
if [ -f filename ]
```

---

### What does `$?` represent?

Exit status of last command.

---
