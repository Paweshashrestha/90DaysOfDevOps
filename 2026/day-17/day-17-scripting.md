---

# Day 17 – Shell Scripting: Loops, Arguments & Error Handling

Shell scripting becomes powerful when you add:

* **Loops → repeat tasks**
* **Arguments → reusable scripts**
* **Error handling → production-safe automation**

DevOps engineers use these for:

* installing software on many servers
* deploying services
* checking system health
* automating CI/CD pipelines

---

# Task 1 – For Loop

A **for loop** repeats a command for a list of items.

Structure:

```bash
for item in list
do
    command
done
```

Think of it like:

```
Take item → run command → take next item → repeat
```

---

# Script 1: Loop Through Fruits

Create:

```
for_loop.sh
```

Code:

```bash
#!/bin/bash

FRUITS="apple banana mango orange grape"

for fruit in $FRUITS
do
    echo "Fruit: $fruit"
done
```

Run:

```bash
chmod +x for_loop.sh
./for_loop.sh
```

Output:

```
Fruit: apple
Fruit: banana
Fruit: mango
Fruit: orange
Fruit: grape
```

Explanation:

```
FRUITS="apple banana mango orange grape"
```

This creates a **list**.

The loop picks **one fruit at a time**.

---

# Script 2 – Print Numbers 1–10

Create:

```
count.sh
```

Code:

```bash
#!/bin/bash

for i in {1..10}
do
    echo $i
done
```

Output:

```
1
2
3
4
5
6
7
8
9
10
```

Explanation:

```
{1..10}
```

Bash generates numbers automatically.

---

# DevOps Example of For Loop

Installing packages:

```bash
for pkg in nginx curl git
do
    apt install -y $pkg
done
```

Instead of writing:

```
apt install nginx
apt install curl
apt install git
```

---

# Task 2 – While Loop

A **while loop** runs **as long as a condition is true**.

Structure:

```bash
while [ condition ]
do
   command
done
```

---

# Script – Countdown

Create:

```
countdown.sh
```

Code:

```bash
#!/bin/bash

read -p "Enter a number: " NUM

while [ $NUM -ge 0 ]
do
    echo $NUM
    NUM=$((NUM-1))
done

echo "Done!"
```

Run:

```
Enter a number: 5
```

Output:

```
5
4
3
2
1
0
Done!
```

---

# Important Concept: Arithmetic

```
NUM=$((NUM-1))
```

This means:

```
NUM = NUM - 1
```

Shell uses:

```
$(( ))
```

for calculations.

Example:

```
echo $((5+3))
```

Output:

```
8
```

---

# DevOps Example of While Loop

Wait for server to start:

```bash
while ! curl -s http://localhost:80
do
   echo "Waiting for server..."
   sleep 5
done
```

Used in **deployment scripts**.

---

# Task 3 – Command-Line Arguments

Arguments allow scripts to **accept input from command line**.

Example:

```
./script.sh value1 value2
```

---

# Special Variables

| Variable | Meaning             |
| -------- | ------------------- |
| `$0`     | script name         |
| `$1`     | first argument      |
| `$2`     | second argument     |
| `$#`     | number of arguments |
| `$@`     | all arguments       |

---

# Script – greet.sh

Create:

```
greet.sh
```

Code:

```bash
#!/bin/bash

if [ -z "$1" ]
then
    echo "Usage: ./greet.sh <name>"
else
    echo "Hello, $1!"
fi
```

Run:

```
./greet.sh Pawesha
```

Output:

```
Hello, Pawesha!
```

Run without argument:

```
./greet.sh
```

Output:

```
Usage: ./greet.sh <name>
```

---

# Important: `-z`

```
-z
```

Checks if variable is **empty**.

---

# Script – args_demo.sh

Create:

```
args_demo.sh
```

Code:

```bash
#!/bin/bash

echo "Script name: $0"
echo "Total arguments: $#"
echo "All arguments: $@"
```

Run:

```
./args_demo.sh apple banana mango
```

Output:

```
Script name: ./args_demo.sh
Total arguments: 3
All arguments: apple banana mango
```

---

# DevOps Example Using Arguments

Deployment script:

```
./deploy.sh production
```

Script:

```bash
ENV=$1

echo "Deploying to $ENV environment"
```

---

# Task 4 – Install Packages via Script

This is a **very real DevOps automation task**.

---

Create:

```
install_packages.sh
```

Code:

```bash
#!/bin/bash

PACKAGES="nginx curl wget"

for pkg in $PACKAGES
do
    if dpkg -s $pkg &> /dev/null
    then
        echo "$pkg is already installed"
    else
        echo "Installing $pkg..."
        apt install -y $pkg
    fi
done
```

---

# Explanation

Check package:

```
dpkg -s nginx
```

If installed:

```
Status: install ok installed
```

We hide output:

```
&> /dev/null
```

Meaning:

```
send output to nowhere
```

---

# Output Example

```
nginx is already installed
Installing curl...
Installing wget...
```

---

# Task 5 – Error Handling

In production scripts **you must handle errors**.

Otherwise automation breaks silently.

---

# Script – safe_script.sh

Create:

```
safe_script.sh
```

Code:

```bash
#!/bin/bash

set -e

mkdir /tmp/devops-test || echo "Directory already exists"

cd /tmp/devops-test || echo "Failed to enter directory"

touch testfile.txt || echo "File creation failed"

echo "Script completed"
```

---

# What `set -e` Does

```
set -e
```

Means:

> Exit script immediately if any command fails.

Example:

If:

```
cd folder_that_does_not_exist
```

Script stops automatically.

This prevents **dangerous automation failures**.

---

# `||` Operator

```
command || alternative
```

Meaning:

```
if command fails → run alternative
```

Example:

```
mkdir test || echo "Directory exists"
```

---

# Check If Script Runs as Root

Add this to `install_packages.sh`.

```bash
if [ "$EUID" -ne 0 ]
then
   echo "Please run as root"
   exit 1
fi
```

Explanation:

```
EUID
```

= Effective User ID.

```
0 = root
```

---

# Full Improved install_packages.sh

```bash
#!/bin/bash

if [ "$EUID" -ne 0 ]
then
    echo "Please run as root"
    exit 1
fi

PACKAGES="nginx curl wget"

for pkg in $PACKAGES
do
    if dpkg -s $pkg &> /dev/null
    then
        echo "$pkg already installed"
    else
        echo "Installing $pkg..."
        apt install -y $pkg
    fi
done
```

---

# Real DevOps Automation Example

Provision server:

```bash
#!/bin/bash

set -e

PACKAGES="nginx git docker.io"

for pkg in $PACKAGES
do
    if ! dpkg -s $pkg &> /dev/null
    then
        apt install -y $pkg
    fi
done

systemctl start nginx
systemctl enable nginx

echo "Server ready!"
```

This is how **infrastructure automation begins**.

---

# What I Learned (3 Key Points)

### 1

**Loops allow automation of repetitive tasks like installing packages or processing files.**

---

### 2

**Command-line arguments make scripts reusable and flexible.**

---

### 3

**Error handling (`set -e`, `||`, checks) prevents scripts from silently failing in production.**

---

# DevOps Interview Questions

### What is `$1` in bash?

The **first command-line argument passed to a script.**

---

### Difference between `$@` and `$#`?

| Variable | Meaning             |
| -------- | ------------------- |
| `$#`     | number of arguments |
| `$@`     | list of arguments   |

---

### What does `set -e` do?

Stops script execution if any command fails.

---

### Why check if script runs as root?

Some commands like:

```
apt install
systemctl start
```

require **root privileges**.

---
