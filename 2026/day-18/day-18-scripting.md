---

# Day 18 – Shell Scripting: Functions & Slightly Advanced Concepts

As scripts grow, they become messy.

Bad script example:

```bash
echo "Checking disk"
df -h

echo "Checking memory"
free -h

echo "Checking uptime"
uptime
```

Hard to maintain.

Instead we use **functions**.

Functions make scripts:

* reusable
* clean
* easier to debug
* modular

Exactly like **functions in Python or JavaScript**.

---

# Task 1 – Basic Functions

## Function Syntax

```bash
function_name() {
    commands
}
```

Call function:

```bash
function_name
```

---

# Script: `functions.sh`

```bash
#!/bin/bash

greet() {
    echo "Hello, $1!"
}

add() {
    SUM=$(($1 + $2))
    echo "Sum is $SUM"
}

greet "Pawesha"
add 5 10
```

---

## Output

```
Hello, Pawesha!
Sum is 15
```

---

## Explanation

### Function Argument

Inside function:

```
$1
```

means **first argument passed to the function**.

Example:

```
greet "Pawesha"
```

Inside greet:

```
$1 = Pawesha
```

---

### Math in Bash

```
$(())
```

Example:

```
SUM=$(($1 + $2))
```

---

# DevOps Example

Restart service:

```bash
restart_service() {
    systemctl restart $1
    echo "$1 restarted"
}

restart_service nginx
restart_service docker
```

Reusable for many services.

---

# Task 2 – Functions with System Checks

Create:

```
disk_check.sh
```

---

# Script

```bash
#!/bin/bash

check_disk() {
    echo "Disk Usage:"
    df -h /
}

check_memory() {
    echo "Memory Usage:"
    free -h
}

echo "Running system checks..."

check_disk
check_memory
```

---

## Output Example

```
Running system checks...

Disk Usage:
Filesystem      Size  Used Avail Use%
/dev/sda1       50G   20G   28G  42%

Memory Usage:
              total        used        free
Mem:           7.7G        2.3G        3.5G
```

---

# Why DevOps Engineers Use Functions

Instead of repeating:

```
df -h
df -h
df -h
```

You create reusable functions.

Example monitoring script:

```
check_disk
check_memory
check_cpu
```

---

# Task 3 – Strict Mode (`set -euo pipefail`)

This is **VERY important for production scripts**.

Add this to the top:

```bash
set -euo pipefail
```

Full script example:

```bash
#!/bin/bash
set -euo pipefail

echo "Strict mode enabled"
```

---

# What Each Flag Does

## `set -e`

Exit script if **any command fails**.

Example:

```bash
mkdir test
cd wrong_directory
echo "Hello"
```

Without `set -e`:

Script continues.

With `set -e`:

Script **stops immediately**.

This prevents **dangerous automation failures**.

---

# `set -u`

Error if **undefined variable is used**.

Example:

```bash
echo $NAME
```

If NAME not defined:

Error:

```
unbound variable
```

Without `set -u`:

Script silently runs.

---

# `set -o pipefail`

Pipelines normally hide errors.

Example:

```
command1 | command2
```

If command1 fails:

Pipeline still succeeds.

With `pipefail`:

Pipeline **fails correctly**.

Example:

```
cat missingfile | grep hello
```

With pipefail:

Script exits.

---

# Example Script

`strict_demo.sh`

```bash
#!/bin/bash
set -euo pipefail

echo "Testing strict mode"

echo $UNDEFINED_VAR
```

Output:

```
unbound variable
```

Script stops.

---

# Why Strict Mode Matters in DevOps

Production automation should **never silently fail**.

Example:

Deployment script:

```
git pull
docker build
docker run
```

If `git pull` fails but script continues → **broken deployment**.

Strict mode prevents this.

---

# Task 4 – Local Variables

Functions should not pollute global variables.

Use:

```
local
```

---

# Script: `local_demo.sh`

```bash
#!/bin/bash

test_local() {
    local MESSAGE="Hello from function"
    echo $MESSAGE
}

test_local

echo $MESSAGE
```

---

## Output

```
Hello from function
(blank)
```

Why?

Because:

```
local MESSAGE
```

Exists **only inside function**.

---

# Without Local

```
MESSAGE="Hello"
```

Variable becomes **global**.

Example:

```bash
test_global() {
    MESSAGE="Inside function"
}

test_global

echo $MESSAGE
```

Output:

```
Inside function
```

The variable leaked outside.

---

# Why Local Variables Matter

Large scripts may contain **many functions**.

Without `local`, variables overwrite each other.

DevOps best practice:

```
Always use local inside functions
```

---

# Task 5 – Build a Real Script

Create:

```
system_info.sh
```

---

# Script

```bash
#!/bin/bash
set -euo pipefail

print_header() {
    echo "=============================="
    echo "$1"
    echo "=============================="
}

system_info() {
    print_header "System Info"
    hostname
    uname -a
}

show_uptime() {
    print_header "Uptime"
    uptime
}

disk_usage() {
    print_header "Disk Usage"
    df -h | sort -hr -k5 | head -n 5
}

memory_usage() {
    print_header "Memory Usage"
    free -h
}

top_cpu() {
    print_header "Top CPU Processes"
    ps -eo pid,comm,%cpu --sort=-%cpu | head -n 6
}

main() {
    system_info
    show_uptime
    disk_usage
    memory_usage
    top_cpu
}

main
```

---

# Example Output

```
==============================
System Info
==============================
devops-server
Linux ubuntu 5.15

==============================
Uptime
==============================
 10:20:33 up 5 days

==============================
Disk Usage
==============================
Filesystem Size Used Use%

==============================
Memory Usage
==============================
total used free

==============================
Top CPU Processes
==============================
PID COMMAND %CPU
```

---

# Why This Script Is DevOps Level

Uses:

- functions
- strict mode
- modular structure
- readable output

This is how **real production scripts are structured**.

---

# Key DevOps Script Structure

Most professional scripts look like this:

```bash
#!/bin/bash
set -euo pipefail

function1() {}
function2() {}
function3() {}

main() {
   function1
   function2
   function3
}

main
```

---

# What I Learned (3 Key Points)

### 1

Functions make scripts **modular, reusable, and easier to maintain**.

---

### 2

`set -euo pipefail` ensures scripts **fail safely and detect hidden errors**.

---

### 3

Using `local` variables prevents **unexpected variable conflicts in larger scripts**.

---

# DevOps Interview Questions

### What does `set -euo pipefail` do?

| Flag       | Meaning                             |
| ---------- | ----------------------------------- |
| `-e`       | exit if command fails               |
| `-u`       | error on undefined variables        |
| `pipefail` | pipeline fails if any command fails |

---

### Why use functions in shell scripts?

Functions improve **code reuse, readability, and maintainability**.

---

### What does `$?` represent?

Exit status of last command.

```
0 = success
non-zero = failure
```

---

### Why use `local` variables?

To prevent variables from **leaking outside functions**.

---
