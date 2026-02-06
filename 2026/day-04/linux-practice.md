# Day 04 – Linux Practice: Processes and Services

Practice note from running basic process, service, and log commands.  
*Run these on your system and paste your actual output below.*

---

## 1. Process checks

### Command 1: List processes (ps aux)
```bash
ps aux | head -15
```
**What it does:** Shows all processes with user, PID, %CPU, %MEM, and command.  
**Note:** Run `ps aux | head -15` and paste your output here.

### Command 2: Find process by name (pgrep)
```bash
pgrep -la sshd
```
**What it does:** Lists PIDs and names of processes matching `sshd`.  
**Note:** Run and paste output (or use another service name like `cron`, `nginx`).
Good 👍 Short and clear explanation:

```bash
pgrep -la sshd
```

### 🔹 What does `-l` mean?

`-l` = **list process name**

Without `-l`, `pgrep` only shows PID.

Example:

```bash
pgrep sshd
```

Output:

```
1234
1250
```

With `-l`:

```bash
pgrep -l sshd
```

Output:

```
1234 sshd
1250 sshd
```

So `-l` shows the **process name next to PID**.

---

### 🔹 What does `-a` mean?

`-a` = **show full command line**

It prints the complete command used to start the process.

Example:

```bash
pgrep -la sshd
```

Output:

```
1234 /usr/sbin/sshd -D
1250 sshd: user@pts/0
```

Now you see:

* PID
* Full command
* Sometimes user session info

---

### 🧠 Memory Trick

* `l` → **label** (show name)
* `a` → **all arguments** (full command)

---

### Final Meaning

```bash
pgrep -la sshd
```

👉 Find sshd processes
👉 Show PID
👉 Show full command


---

## 2. Service checks

**Target service inspected:** `sshd` (SSH daemon)

### Command 3: Service status (systemctl status)
```bash
systemctl status sshd
```
**What it does:** Shows whether the service is active, enabled, and recent log lines.  
**Note:** Run and paste your output. Look for "active (running)" or "inactive (dead)".

### Command 4: List loaded service units (systemctl list-units)
```bash
systemctl list-units --type=service --state=running | head -20
```
**What it does:** Lists currently running systemd services.  
**Note:** Run and paste first 20 lines to see what’s active on your system.

---


```bash
systemctl list-units --type=service --state=running | head -20
```

---

# 🔎 Full Meaning (Broken Down)

## 1️⃣ `systemctl`

Controls and inspects **systemd** (Linux service manager).

Think:

> systemctl = service control center

---

## 2️⃣ `list-units`

Shows all active systemd units.

A **unit** can be:

* service
* socket
* mount
* timer
* target

---

## 3️⃣ `--type=service`

Filter only **services**.

Without this, you would see:

* mounts
* sockets
* targets
* devices

We only want background services.

---

## 4️⃣ `--state=running`

Show only services that are currently:

> Active and running

It hides:

* inactive
* failed
* dead
* exited

---

## 5️⃣ `| head -20`

Limit output to first 20 lines.

Because production servers may have 100+ services running.

---

# 🧠 What Output Looks Like

You’ll see something like:

```
UNIT                 LOAD   ACTIVE SUB     DESCRIPTION
dbus.service         loaded active running D-Bus System Message Bus
sshd.service         loaded active running OpenSSH server daemon
systemd-journald.service loaded active running Journal Service
```

---

# 📌 Important Columns Explained

| Column      | Meaning              |
| ----------- | -------------------- |
| UNIT        | Service name         |
| LOAD        | Loaded successfully? |
| ACTIVE      | High-level state     |
| SUB         | Detailed state       |
| DESCRIPTION | What service does    |

---

# 🔥 Why DevOps Engineers Use This

When debugging:

* App not working?
* Server slow?
* Service missing?

First check:

```bash
systemctl list-units --type=service --state=running
```

It quickly tells:

* What is running
* What is not running

---

# 🧠 Simple Memory Trick

* `--type=service` → Only services
* `--state=running` → Only active ones
* `head -20` → Just preview

Think:

> “Show me top 20 running services.”

---

## 3. Log checks

### Command 5: Journal logs for the service (journalctl)
```bash
journalctl -u sshd -n 30 --no-pager
```
**What it does:** Last 30 log lines for the `sshd` unit.  
**Note:** Run and paste a short snippet. Look for "Accepted" (logins) or errors.

### Command 6: Last lines of a log file (tail)
```bash
tail -n 20 /var/log/syslog
```
**What it does:** Last 20 lines of system log. (On RHEL/Fedora use `/var/log/messages` if syslog is not present.)  
**Note:** Run and paste output. Adjust path for your distro.


---

# 🔹 Command 5: `journalctl`

```bash
journalctl -u sshd -n 30 --no-pager
```

## 🧠 What is `journalctl`?

`journalctl` reads logs from **systemd journal**.

Modern Linux systems (Ubuntu, Fedora, RHEL, Arch, etc.) use **systemd**, and systemd stores logs in a centralized binary journal.

Think:

> journalctl = log viewer for systemd services

---

## 🔍 Break It Down

### 1️⃣ `-u sshd`

* `-u` = unit
* Shows logs only for the **sshd service**

Without `-u`, you would see logs from entire system.

---

### 2️⃣ `-n 30`

* `-n` = number of lines
* Show last 30 log entries

Like:

* `tail -n 30`

---

### 3️⃣ `--no-pager`

By default, journalctl opens in pager mode (like `less`).

`--no-pager`:

* Prints directly to terminal
* Useful in scripts or quick checks

---


# 🔹 What is a Pager?

A **pager** is a program that shows long output **page by page**.

Common pager in Linux:

```bash
less
```

When output is very long, instead of printing everything at once, it opens like this:

* You scroll with ↑ ↓
* Press `q` to quit

---

# 🔹 What Happens Without `--no-pager`

If you run:

```bash
journalctl -u sshd
```

It will open inside `less`.

You must:

* Press `Space` to scroll
* Press `q` to exit

This is good for manual reading.

---

# 🔹 What `--no-pager` Does

```bash
journalctl -u sshd --no-pager
```

It disables pager.

👉 Output prints directly in terminal
👉 No scrolling interface
👉 No need to press `q`

It just dumps logs and returns to prompt.

---

# 🔹 Why It Is Important

### ✅ Useful in scripts

If you write:

```bash
journalctl -u sshd --no-pager > ssh-log.txt
```

It works cleanly.

Without `--no-pager`, script may hang waiting for pager exit.

---

### ✅ Useful in automation / CI/CD

DevOps tools don’t interact with pagers.
So we disable it.

---

# 🧠 Simple Memory Trick

Pager = page viewer
`--no-pager` = “Don’t open viewer, just print”

---

# 🎯 When to Use What

| Situation            | Use                     |
| -------------------- | ----------------------- |
| Manual debugging     | Default (pager is fine) |
| Script / automation  | `--no-pager`            |
| Quick terminal check | `--no-pager`            |

---


## 🧾 What You Should Look For

If checking SSH:

### ✅ Successful login:

```
Accepted password for user from 192.168.1.10 port 53422 ssh2
```

### ❌ Failed login:

```
Failed password for user from 192.168.1.10 port 53422 ssh2
```

### ❌ Error example:

```
error: Bind to port 22 failed
```

---

## 🚀 Why This Is Powerful

If service fails:

```bash
systemctl status sshd
```

Then immediately:

```bash
journalctl -u sshd -n 50
```

Logs will tell:

* Permission issue
* Port already used
* Config error
* Authentication failure

Logs = root cause.

---

# 🔹 Command 6: `tail`

```bash
tail -n 20 /var/log/syslog
```

## 🧠 What is `tail`?

`tail` shows the **end of a file**.

Because logs keep growing, newest messages are at bottom.

---

## 🔍 Break It Down

### 1️⃣ `-n 20`

Show last 20 lines.

### 2️⃣ `/var/log/syslog`

This is the **main system log file** (Debian/Ubuntu).

On:

* RHEL/CentOS/Fedora → `/var/log/messages`

---

## 🔥 Why Use tail?

Sometimes:

* Service logs also go to syslog
* Kernel logs appear here
* System errors appear here

---

## 👀 Real DevOps Usage

Live monitoring:

```bash
tail -f /var/log/syslog
```

`-f` = follow

It shows logs in real-time.

Very useful when:

* Restarting service
* Testing login
* Debugging crash

---

# 🧠 Difference Between journalctl and tail

| journalctl               | tail                              |
| ------------------------ | --------------------------------- |
| Reads systemd journal    | Reads plain text file             |
| Filter by service (`-u`) | Cannot filter by service directly |
| Structured logging       | Raw file view                     |
| Modern systems           | Older log systems                 |

---

# 🎯 Practical Troubleshooting Example

Problem: SSH not working

Step 1:

```bash
systemctl status sshd
```

Step 2:

```bash
journalctl -u sshd -n 50
```

Step 3:

```bash
tail -n 50 /var/log/syslog
```

Step 4:
Check port:

```bash
ss -tlnp | grep 22
```

---

# 🧠 Simple Memory Trick

* `journalctl` → service logs
* `tail` → file logs
* `-n` → number of lines
* `-f` → follow live

---

Excellent 👏 this is a **very important DevOps command**.

Let’s break it clearly and deeply.

---

# 🔹 Full Command

```bash
ss -tlnp | grep 22
```

Goal:

👉 Check if **port 22 (SSH)** is listening
👉 See which process is using it

---

# 🧠 Step 1: What is `ss`?

`ss` = **Socket Statistics**

It shows:

* Open network connections
* Listening ports
* Associated processes

It is modern replacement for `netstat`.

---

# 🧩 Step 2: Break the Flags

```bash
ss -tlnp
```

### 🔹 `-t`

Show **TCP** connections only
(SSH uses TCP)

---

### 🔹 `-l`

Show only **listening** sockets

Listening means:

> Service is waiting for incoming connections

Example:
SSH waits on port 22

---

### 🔹 `-n`

Show **numeric values**

Without `-n`:

* It tries to convert port numbers into service names
* Slower

With `-n`:

* Shows raw port numbers (like 22, 80, 443)

---

### 🔹 `-p`

Show **process using the port**

Very important.

It shows:

* PID
* Program name

⚠ Usually requires `sudo` to see all processes.

---

# 🔎 Step 3: `| grep 22`

Pipe output to search for:

```bash
grep 22
```

Why 22?

Because:

> SSH runs on port 22

---

# 🧾 Example Output

```bash
LISTEN 0 128 0.0.0.0:22 0.0.0.0:* users:(("sshd",pid=1234,fd=3))
```

---

# 🔍 What This Means

| Part                | Meaning                        |
| ------------------- | ------------------------------ |
| LISTEN              | Port is waiting for connection |
| 0.0.0.0:22          | Listening on all interfaces    |
| users:(("sshd"...)) | sshd process owns this port    |
| pid=1234            | Process ID                     |

---

# 🔥 If Nothing Shows

If command returns nothing:

```bash
( empty )
```

It means:
❌ Nothing is listening on port 22
→ SSH is not running
→ Or firewall blocked
→ Or service failed

---

# 🧠 Real Troubleshooting Flow

If SSH not working:

1️⃣ Check service:

```bash
systemctl status sshd
```

2️⃣ Check port:

```bash
ss -tlnp | grep 22
```

3️⃣ Check logs:

```bash
journalctl -u sshd
```

---

# 🎯 Why This Command Is Powerful

It answers 3 critical questions:

* Is port open?
* Which service is using it?
* Is it listening on all interfaces?

---

# 🧠 Memory Trick

`t` → TCP
`l` → Listening
`n` → Numeric
`p` → Process

Think:

> “Show TCP listening ports with process.”

---

# 🚀 Bonus (Interview-Level)

Better version:

```bash
sudo ss -tlnp | grep :22
```

Why `:22`?

Prevents matching things like:

* 2200
* 5222

More precise filtering.

---

If you want next level, I can explain:

* Difference between `0.0.0.0:22` and `127.0.0.1:22`
* Difference between LISTEN and ESTAB
* How firewall interacts with listening ports
* How sockets actually work internally in Linux

Tell me your depth 🚀


## 4. Mini troubleshooting flow

**Goal:** Confirm SSH service is running and see recent activity.

| Step | Command | Purpose |
|------|---------|---------|
| 1 | `systemctl status sshd` | Is the service up? |
| 2 | `pgrep -la sshd` | Which PIDs are sshd? |
| 3 | `journalctl -u sshd -n 50` | Any errors or recent logins? |
| 4 | `ss -tlnp \| grep 22` or `ss -tlnp` | Is port 22 listening? |

**Summary:** Always check status first, then process list, then logs, then network (port). Replace `sshd` with your target service (e.g. `nginx`, `docker`, `cron`).

---

*Replace the "paste your output" placeholders above with real output from your machine after running each command.*
