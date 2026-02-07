---

# Day 05 – Linux Troubleshooting Runbook (Deep Version with Examples)

## Target service: `sshd` (SSH daemon)

### Why `sshd`?

* SSH is the **primary remote access method** for Linux servers.
* If SSH fails:

  * You lose remote access
  * Automation (Ansible, CI/CD, Git pulls) may fail
  * Production response time increases

### 🔎 Example Situation

Your team reports:

> “We cannot SSH into prod-web-01.”

That immediately becomes a **high-priority incident**, because without SSH:

* You can’t deploy
* You can’t investigate logs
* You can’t restart services

---

# What “Health Snapshot” Really Means

A **health snapshot** is:

> A structured, quick collection of system state data at a specific moment in time.

It answers:

- Is the system overloaded?
- Is disk full?
- Is memory exhausted?
- Is the service running?
- Is the port listening?
- Are there errors in logs?

### 🔎 Example

You run:

```bash
df -h
free -h
systemctl status sshd
```

In 30 seconds, you understand:

- Disk is 100% full
- Memory is fine
- SSH is inactive

That is your **snapshot**.

---

# 1️⃣ Target Service / Process

- Service: `sshd`
- Systemd unit: `sshd.service`
- Default port: 22
- Log locations:
  - Debian/Ubuntu → `/var/log/auth.log`
  - RHEL/CentOS → `/var/log/secure`
  - systemd → `journalctl -u sshd`

### 🔎 Example

On Ubuntu:

```bash
tail -n 20 /var/log/auth.log
```

On RHEL:

```bash
tail -n 20 /var/log/secure
```

Wrong OS = wrong log file = wasted time.

---

# 2️⃣ Snapshot: Environment Basics

These commands verify you are on the correct machine.

---

## 1. `uname -a`

```bash
uname -a
```

### Example Output

```bash
Linux prod-web-01 5.15.0-91-generic x86_64 GNU/Linux
```

### What it confirms

- Hostname: `prod-web-01`
- Kernel: `5.15`
- Architecture: `x86_64`

### 🚨 Example Incident

You think you're on production.

But output shows:

```bash
Linux staging-web-01 5.15.0-91-generic x86_64 GNU/Linux
```

You're on staging.

If you restart sshd here thinking it's prod — that’s a serious mistake.

---

## 2. `cat /etc/os-release`

```bash
cat /etc/os-release
```

### Example Output

```bash
NAME="Ubuntu"
VERSION="22.04.3 LTS"
```

### Why this matters

If OS = Ubuntu:

- Logs → `/var/log/auth.log`
- Package manager → `apt`

If OS = RHEL:

- Logs → `/var/log/secure`
- Package manager → `yum` or `dnf`

### 🔎 Real Example

You try:

```bash
tail -n 50 /var/log/auth.log
```

Error:

```
No such file or directory
```

You check OS → it’s RHEL.

Correct command:

```bash
tail -n 50 /var/log/secure
```

Problem solved quickly.

---

# 3️⃣ Snapshot: Filesystem Sanity

Disk full = services crash.

---

## 3. Write Test

```bash
mkdir -p /tmp/runbook-demo
cp /etc/hosts /tmp/runbook-demo/hosts-copy
ls -l /tmp/runbook-demo
```

### Healthy Output

```bash
-rw-r--r-- 1 root root 215 Jun 3 10:20 hosts-copy
```

### 🚨 Failure Example

```bash
cp: cannot create regular file: No space left on device
```

This means:

- Disk is full
- Services may fail
- Logs cannot be written

SSH might stop working because it cannot write authentication logs.


---

# ✅ Part 1: `df -h`

## What is `df`?

`df` = **Disk Free**

It shows:

> How much disk space is used and how much is left.

---

## What does `-h` mean?

`-h` = **human readable**

Without `-h`, you see numbers like:

```
20485760
```

With `-h`, you see:

```
20G
```

Much easier to read.

---

## Command

```bash
df -h
```

---

## Output Explained (Simple)

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   27G  43% /
/dev/sda2        20G    6G   13G  32% /var
```

Let’s break each column:

| Column     | Simple Meaning              |
| ---------- | --------------------------- |
| Filesystem | Disk name                   |
| Size       | Total disk size             |
| Used       | How much is used            |
| Avail      | How much free space left    |
| Use%       | Percentage used             |
| Mounted on | Where this disk is attached |

---

## What is `/dev/sda1`?

* `/dev/` = device
* `sd` = storage disk
* `a` = first disk
* `1` = first partition

Think of it like:

Disk 1 → Part 1

---

## What is “Mounted on”?

Linux attaches disks to folders.

Example:

```
/dev/sda1 → /
/dev/sda2 → /var
```

That means:

* `/` is main system
* `/var` stores logs and data

---

# 🚨 Dangerous Example Explained

```bash
/dev/sda2  20G  20G  0  100%  /var
```

What this means:

* Total size = 20G
* Used = 20G
* Available = 0
* 100% full

Zero space left.

---

# Why is `/var` important?

`/var` stores:

* Logs
* SSH login records
* Application data
* Package downloads

If `/var` is 100%:

* Logs cannot be written
* SSH may fail
* Services may not start

---

# ✅ Part 2: CPU & Memory

Now this command:

```bash
ps -o pid,pcpu,pmem,comm -p $(pgrep -d, sshd)
```

I know this looks scary 😅
Let’s break it down.

---

# 🔹 Step 1: What is `ps`?

`ps` = **Process Status**

It shows running programs.

---

# 🔹 What is `pgrep sshd`?

`pgrep` = process grep
It finds process IDs (PID).

Example:

```bash
pgrep sshd
```

Output:

```
1234
```

That means sshd has PID 1234.

---

# 🔹 What is `$( ... )` ?

This is called:

> Command substitution

It means:
“Run the inside command first.”

So:

```bash
$(pgrep sshd)
```

Becomes:

```
1234
```

---

# 🔹 What is `-p` ?

`-p` = show specific PID

So:

```bash
ps -p 1234
```

Shows only sshd.

---

# 🔹 What is `-o pid,pcpu,pmem,comm` ?

`-o` = output format

We are telling ps:

“Only show these columns.”

| Word | Meaning        |
| ---- | -------------- |
| pid  | Process ID     |
| pcpu | CPU usage %    |
| pmem | Memory usage % |
| comm | Command name   |

---

# 🔥 So What Is The Full Command Doing?

```bash
ps -o pid,pcpu,pmem,comm -p $(pgrep -d, sshd)
```

It means:

1. Find sshd process ID
2. Show only that process
3. Show PID, CPU %, Memory %, Command

---

# ✅ Normal Example

```bash
PID %CPU %MEM COMMAND
1234  0.0  0.1 sshd
```

Meaning:

* sshd is using almost no CPU
* Memory is very low
* Everything normal

---

# 🚨 Bad Example

```bash
PID %CPU %MEM COMMAND
1234 95.0  5.2 sshd
```

Meaning:

* CPU usage is 95%
* That is extremely high

Possible reasons:

* Brute-force attack
* Too many SSH connections
* Misconfiguration

---

# 🧠 Difficult Words Explained

| Word           | Simple Meaning                     |
| -------------- | ---------------------------------- |
| Filesystem     | Disk or partition                  |
| Partition      | Part of a disk                     |
| Mounted        | Attached to a folder               |
| PID            | Process ID number                  |
| CPU            | Processor usage                    |
| Memory (RAM)   | Working memory of system           |
| Resource usage | How much CPU or RAM a program uses |

---

# 💡 Simple Mental Model

Think of server like a house:

* Disk = storage room
* RAM = working table
* CPU = brain
* sshd = door for remote access

If:

* Storage room full → can’t store logs
* Table full → system slow
* Brain overloaded → system unstable
* Door process broken → can’t enter house

---


# ✅ What is `free -h`?

## 🔹 `free`

`free` = shows **memory (RAM) usage** on your system.

It tells you:

* How much RAM you have
* How much is used
* How much is free
* How much is available

---

## 🔹 What does `-h` mean?

`-h` = **human readable**

Without `-h`, memory shows in bytes (hard to read).

With `-h`, it shows:

* `Gi` → Gigabytes
* `Mi` → Megabytes

Much easier.

---

# Command

```bash
free -h
```

---

# 📊 Output Explained

### Healthy Example

```bash
              total   used   free  available
Mem:           8Gi    4Gi    1Gi     3Gi
```

Let’s explain each column in simple words:

| Column    | Meaning (Simple)                  |
| --------- | --------------------------------- |
| total     | Total RAM installed               |
| used      | RAM currently used                |
| free      | Completely unused RAM             |
| available | RAM that can still be used safely |

---

# 🔹 What is “available”?

This is the most important column.

Linux uses memory for:

* Running programs
* Caching files (to speed things up)

So even if “free” looks small, memory may still be usable.

👉 That’s why **available** is more important than “free”.

---

# 🚨 Dangerous Example

```bash
Mem:           8Gi    7.8Gi   50Mi    20Mi
```

What this means:

* Total RAM = 8GB
* Almost all is used
* Only 20MB available

20MB is extremely low.

---

# What Happens When Memory Is Low?

### 1️⃣ System Starts Swapping

Swapping = moving memory data from RAM → disk.

Disk is much slower than RAM.

Result:

* System becomes slow
* SSH login takes time
* Commands lag

---

### 2️⃣ Services Slow Down

Programs need RAM to work.

If no memory:

* Applications freeze
* Database slows
* Web server delays

---

### 3️⃣ OOM Killer May Activate

OOM = Out Of Memory

Linux has something called:

> OOM Killer

When system runs out of memory:

* Kernel automatically kills a process
* It chooses the process using most memory

Example log:

```bash
Out of memory: Kill process 1234 (java)
```

If unlucky:

* SSH process (`sshd`) could be killed
* You lose connection suddenly

---

# 🧠 Simple Analogy

Think of RAM like a work desk.

* If desk has space → you work fast
* If desk is full → you struggle
* If desk completely full → you throw something away (OOM killer)

---

# 🔥 Why This Matters in SSH Troubleshooting

If users say:

> "SSH keeps disconnecting"

You check:

```bash
free -h
```

You see:

```
available 20Mi
```



You are seeing units like:

* **GB**
* **Gi**
* **MB**
* **Mi**

Let’s break it down very clearly.

---

# 1️⃣ MB vs MiB

### 🔹 MB = Megabyte (Decimal system)

* 1 MB = **1,000,000 bytes**
* Based on powers of **10**

Used by:

* Hard drive manufacturers
* Internet speed

---

### 🔹 MiB = Mebibyte (Binary system)

* 1 MiB = **1,048,576 bytes**
* Based on powers of **2**
* 1 MiB = 1024 KiB
* 1 KiB = 1024 bytes

Used by:

* Linux
* `free -h`
* `top`
* Memory tools

---

# 2️⃣ GB vs GiB

### 🔹 GB = Gigabyte (Decimal)

* 1 GB = 1,000,000,000 bytes

### 🔹 GiB = Gibibyte (Binary)

* 1 GiB = 1,073,741,824 bytes

---

# 3️⃣ Which Is Bigger?

For the *same number*:

* **1 GiB is bigger than 1 GB**
* **1 MiB is bigger than 1 MB**

Because binary (GiB, MiB) uses 1024.
Decimal (GB, MB) uses 1000.

---

# 4️⃣ Size Order (Small → Large)

```
MB < MiB < GB < GiB
```

But be careful ⚠️

That comparison assumes same base number.

Example:

* 1000 MB ≈ 0.95 GiB
* 1 GiB ≈ 1.07 GB

---

# 5️⃣ Why Linux Shows GiB

Linux memory works in powers of 2.

RAM is allocated in binary chunks:

* 2^10
* 2^20
* 2^30

So tools like:

```bash
free -h
```

show:

```
8Gi
```

Not 8GB.

Because it’s technically **Gibibytes**, not Gigabytes.

---

# 6️⃣ Simple Rule To Remember

| If You See | It Means | Based On |
| ---------- | -------- | -------- |
| MB / GB    | Decimal  | 1000     |
| Mi / Gi    | Binary   | 1024     |

---

# 🔥 Real-World Example

You buy a "500 GB" hard disk.

Linux may show:

```
465 GiB
```

You think:
"Where did my storage go?? 😨"

Nothing is missing.

It’s just:

* Manufacturer used GB (1000 system)
* Linux shows GiB (1024 system)

---



# 🧠 Quick Rule for Beginners

| Available RAM | Status    |
| ------------- | --------- |
| > 1GB         | Safe      |
| 500MB – 1GB   | Watch     |
| < 200MB       | Dangerous |
| < 50MB        | Critical  |

(General guideline, depends on server size.)

---

# 🎯 Final Simple Definition

`free -h` shows:

> How much memory (RAM) your system has and how much is still safe to use.

Low available memory = system instability risk.


---
Good 👏 this is **real incident-level thinking** now.

You’re not just running commands — you’re diagnosing a system.

I’ll explain everything slowly, including difficult words.

---

# 5️⃣ Snapshot: Disk & I/O (In Depth)

## 🔹 What is “Disk & I/O”?

### Disk

Your storage device (HDD/SSD).
HDD → Hard Disk Drive

SSD → Solid State Drive
### I/O

Input/Output.

In this case:

* Writing logs to disk
* Reading files from disk

If disk is full or slow → system becomes unstable.

---

# Step 1️⃣

## `du -sh /var/log`

```bash
du -sh /var/log
```

Let’s break this completely.

### 🔹 `du`

Disk Usage.

Shows how much space files are using.

### 🔹 `-s`

Summary mode.

Without `-s`, it lists every file recursively.

With `-s`, it shows only total size.

### 🔹 `-h`

Human readable.

Shows:

* K (Kilobyte)
* M (Megabyte)
* G (Gigabyte)

Instead of raw bytes.

---

## Example Output

```bash
8.5G /var/log
```

Meaning:

The directory `/var/log` is using **8.5 Gigabytes**.

That is large.

---

# What is `/var/log`?

### `/var`

Variable data directory.

Stores:

* Logs
* Mail queues
* Package cache
* Runtime data

### `/var/log`

System log files.

Examples:

* Authentication logs
* System boot logs
* Service logs

---

# Step 2️⃣

## Investigate Inside

```bash
du -sh /var/log/*
```

The `*` means:

"All files and folders inside `/var/log`"

Now it shows size of each file.

Example:

```bash
7.8G /var/log/auth.log
300M /var/log/messages
200M /var/log/secure
```

---

# 🚨 You Discover:

```bash
7.8G /var/log/auth.log
```

That is massive.

---

# What is `auth.log`?

Authentication log.

Records:

* SSH login attempts
* Failed passwords
* Sudo usage
* User switching

---

# Why Would It Be 7.8G?

Most common reason:

Brute-force attack.

---

## What is a Brute-force Attack?

An attacker:

* Tries thousands of passwords
* Rapidly attempts SSH login
* Generates log entry for each failure

Each failed attempt writes a log line.

Millions of attempts → huge file.

---

# Step 3️⃣

## Check Partition Space

```bash
df -h /var
```

Let’s break it.

### 🔹 `df`

Disk filesystem.

Shows:

* Total size
* Used
* Available
* Percentage used

### 🔹 `-h`

Human readable.

---

## Example Output

```bash
/dev/sda2 20G 19G 500M 98% /var
```

Break this down:

| Field     | Meaning         |
| --------- | --------------- |
| /dev/sda2 | Disk partition  |
| 20G       | Total size      |
| 19G       | Used            |
| 500M      | Free space      |
| 98%       | Used percentage |
| /var      | Mount point     |

---

# 🚨 Why 98% Is Dangerous

When disk reaches:

* 100% → system can break
* 95%+ → very risky

---

# What Happens at 100%?

1️⃣ Logs cannot be written
2️⃣ systemd may fail
3️⃣ Package installs fail
4️⃣ Database crashes
5️⃣ SSH may stop working

Why?

Because Linux needs free space to:

* Write temporary files
* Write process data
* Write lock files

No space = no writing.

---

# 🔥 Real Incident Scenario

1. Brute-force attack fills `auth.log`
2. `/var` reaches 100%
3. SSH tries to write authentication log
4. Write fails
5. SSH disconnects users

Admin loses access.

This is common in real production systems.

---

# 🧠 Why This Is Called “Snapshot”

You are taking:

* Quick disk usage check
* Quick log growth check
* Quick partition health check

It gives you a **health picture** of system state.

Like a medical vital check:

* Temperature
* Heart rate
* Blood pressure

Here:

* Disk %
* Log size
* Partition free space

---

# 🎯 Interview-Level Understanding

If interviewer asks:

“How do you quickly check if disk issue is causing SSH failure?”

You answer:

1. `df -h` → Check full partition
2. `du -sh /var/log/*` → Identify large log
3. Inspect suspicious log
4. Rotate or truncate logs

That shows operational maturity.

---

# 6️⃣ Snapshot: Network

---

## 9. `ss -tulpn | grep ':22'`

```bash
ss -tulpn | grep ':22'
```

### Healthy Example

```bash
LISTEN 0 128 0.0.0.0:22 0.0.0.0:* users:(("sshd",pid=1234))
```

Meaning:

- SSH is listening
- On all interfaces

### 🚨 Problem Example

No output.

That means:

- sshd not running
- Or listening on different port

Another case:

```bash
LISTEN 0 128 127.0.0.1:22
```

SSH only listening on localhost — remote users cannot connect.

---

# 7️⃣ Logs Reviewed

Logs show the truth.

---

## 11. `journalctl -u sshd -n 50 --no-pager`

```bash
journalctl -u sshd -n 50 --no-pager
```

### Example Error

```bash
error: Bind to port 22 failed: Address already in use
```

This means:
Another service is using port 22.

---

## 12. Auth Log Tail

```bash
tail -n 50 /var/log/auth.log
```

### Brute Force Example

```bash
Failed password for invalid user admin from 192.168.1.50
Failed password for invalid user root from 192.168.1.50
Failed password for invalid user test from 192.168.1.50
```

If thousands appear → active attack.

---

# 8️⃣ Understanding the Flow (With Example)

### Scenario:

Users cannot SSH.

### Your Investigation

1️⃣ Check disk:

```bash
df -h
```

→ `/var` 100%

2️⃣ Check logs size:

```bash
du -sh /var/log
```

→ 9GB auth.log

3️⃣ Clean logs safely

4️⃣ Restart sshd:

```bash
systemctl restart sshd
```

Problem fixed.

Root cause: Log growth filled disk.

---

# 9️⃣ Example Real Incident (Port Conflict)

Users say SSH not working.

You run:

```bash
systemctl status sshd
```

→ Failed

Then:

```bash
journalctl -u sshd
```

Shows:

```
Address already in use
```

You check:

```bash
ss -tulpn | grep 22
```

Output:

```bash
users:(("custom-app",pid=2222))
```

Another app is using port 22.

Resolution:

- Stop conflicting app
- Or change SSH port

---

# 10️⃣ Why This Runbook Matters in DevOps

In production:

- You cannot panic
- You cannot guess
- You must follow structure
- You must document findings

Example Documentation:

```
Incident: SSH outage
Time detected: 10:05 AM
Root cause: /var disk full
Fix: Cleaned logs, restarted sshd
Prevention: Configure logrotate + alert at 80%
```

That’s professional troubleshooting.

---

# Final Mental Model

Think in Layers:

1️⃣ Machine health (CPU, RAM, Disk)
2️⃣ Service state (systemctl)
3️⃣ Process existence (ps/pgrep)
4️⃣ Network binding (ss)
5️⃣ Logs (journalctl, auth.log)

If all 5 are healthy → SSH is healthy.

---
