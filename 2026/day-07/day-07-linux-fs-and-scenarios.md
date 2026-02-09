Think of the Linux filesystem like a **tree**.

```
        /
   ├── bin
   ├── etc
   ├── home
   ├── root
   ├── var
   ├── tmp
   ├── usr
   └── opt
```

Everything in Linux **starts from `/` (root)**.

---

# 1. `/` (Root Directory)

### Meaning

`/` is the **top of the entire Linux filesystem**.

Everything lives **inside it**.

Example:

```
/home
/etc
/var
/usr
```

### Think of it like

The **C:\ drive in Windows**, but more structured.

### Command

```bash
ls /
```

Example output:

```
bin  etc  home  root  tmp  usr  var  opt
```

### Real DevOps Use

Example: navigating to a directory

```
cd /var/log
```

This is an **absolute path** because it starts from `/`.

---

# 2. `/home` (User Files)

### Meaning

`/home` stores **normal users' files**.

Each user gets a folder.

Example:

```
/home/john
/home/pawesha
/home/admin
```

### Inside a user folder

```
/home/pawesha
 ├── Documents
 ├── Downloads
 ├── .bashrc
 └── projects
```

Notice:

```
.bashrc
```

Files starting with `.` are **hidden config files**.

### Command

```bash
ls /home
```

Example:

```
john
pawesha
```

### Real DevOps Use

Check user data:

```bash
du -sh /home/*
```

Example output:

```
2.1G /home/john
350M /home/pawesha
```

Meaning **how much disk each user is using**.

---

# 3. `/root` (Admin Home)

### Meaning

This is the **home directory for the root user**.

Important:

```
root user ≠ normal user
```

So Linux keeps it **separate from `/home`**.

Example:

```
/root
 ├── .bashrc
 ├── scripts
 └── backup.sh
```

### Command

```bash
ls /root
```

You need **sudo or root access**.

Example:

```bash
sudo ls /root
```

### Real DevOps Use

Sometimes admins run scripts like:

```
/root/backup.sh
/root/deploy.sh
```

---

# 4. `/etc` (System Configuration)

### Meaning

`/etc` contains **all system configuration files**.

If Linux behaves wrong → check `/etc`.

### Important files

| File            | Purpose       |
| --------------- | ------------- |
| `/etc/passwd`   | user accounts |
| `/etc/group`    | groups        |
| `/etc/hostname` | system name   |
| `/etc/hosts`    | local DNS     |
| `/etc/ssh/`     | SSH config    |
| `/etc/nginx/`   | Nginx config  |

### Example

```bash
cat /etc/hostname
```

Output:

```
server01
```

### Another Example

```bash
cat /etc/passwd
```

Output example:

```
root:x:0:0:root:/root:/bin/bash
pawesha:x:1000:1000::/home/pawesha:/bin/bash
```

Meaning:

```
username : password : UID : GID : home directory : shell
```

### Real DevOps Use

Example: SSH config

```
/etc/ssh/sshd_config
```

Restart SSH after change:

```bash
sudo systemctl restart ssh
```

---

# 5. `/var` (Variable Data)

### Meaning

`/var` stores **data that keeps changing**.

Example:

```
logs
cache
mail
database files
```

### Important subfolders

```
/var/log
/var/cache
/var/lib
```

---

# 6. `/var/log` (Logs – VERY IMPORTANT)

Logs are **the most important directory for debugging**.

### Example

```bash
ls /var/log
```

Output:

```
syslog
auth.log
kern.log
nginx
apache2
```

### Important logs

| File       | Purpose         |
| ---------- | --------------- |
| `syslog`   | system messages |
| `auth.log` | login attempts  |
| `kern.log` | kernel logs     |

---

### Example: Check login failures

```bash
cat /var/log/auth.log
```

Example output:

```
Failed password for root from 192.168.1.10
```

Meaning **someone tried to hack SSH**.

---

### Real DevOps Scenario

Disk full.

Check logs size:

```bash
du -sh /var/log/*
```

Example:

```
7.8G auth.log
200M syslog
```

Meaning **auth.log is huge**.

Fix:

```bash
sudo truncate -s 0 /var/log/auth.log
```

---

# 7. `/tmp` (Temporary Files)

### Meaning

Temporary files created by programs.

Usually **cleared after reboot**.

### Example

```
/tmp/tmpA82d
/tmp/install.log
```

### Command

```bash
ls /tmp
```

### Important permission

```
drwxrwxrwt
```

Meaning:

```
everyone can write
```

But users **cannot delete other users files**.

---

### Real DevOps Use

Testing scripts:

```bash
nano /tmp/test.sh
```

Run:

```bash
bash /tmp/test.sh
```

---

# 8. `/bin` (Essential Commands)

`/bin` contains **critical commands required for system operation**.

Examples:

```
ls
cp
mv
rm
cat
bash
```

### Command

```bash
ls /bin
```

Example output:

```
ls
cp
mv
bash
cat
```

---

### Why important?

If `/bin` is broken → **Linux cannot function**.

---

# 9. `/usr/bin` (Most Programs)

This directory contains **most installed software commands**.

Example programs:

```
python3
git
curl
vim
node
```

### Command

```bash
ls /usr/bin
```

You may see **1000+ commands**.

---

### Real DevOps Example

Check where python is installed:

```bash
which python3
```

Output:

```
/usr/bin/python3
```

---

# 10. `/opt` (Optional Software)

Used for **third-party applications**.

Example:

```
/opt/docker
/opt/google
/opt/custom-app
```

Many companies install software here.

Example structure:

```
/opt/myapp
   ├── bin
   ├── config
   └── logs
```

---

# Simple Way to Remember Everything

Think of Linux like this:

| Directory  | Meaning              |
| ---------- | -------------------- |
| `/`        | root of everything   |
| `/home`    | user files           |
| `/root`    | admin home           |
| `/etc`     | configuration        |
| `/var`     | changing data        |
| `/var/log` | logs                 |
| `/tmp`     | temporary files      |
| `/bin`     | essential commands   |
| `/usr/bin` | installed programs   |
| `/opt`     | third-party software |

---

# Real DevOps Interview Scenario

### Question

Server disk full.

### Investigation

Step 1

```bash
df -h
```

Step 2

```
cd /var
du -sh *
```

Step 3

```
du -sh /var/log/*
```

Example result

```
8G auth.log
```

Step 4

Fix log:

```bash
truncate -s 0 /var/log/auth.log
```

---

# One Trick to Memorize the Filesystem

Remember this order:

```
/etc → config
/home → users
/var → logs
/tmp → temporary
/bin → core commands
/usr/bin → programs
/opt → external apps
```

---

## Hands-on task (run on your system)

```bash
# Largest log files in /var/log
du -sh /var/log/* 2>/dev/null | sort -h | tail -5

# Sample config
cat /etc/hostname

# Your home directory
ls -la ~
```

---

# Part 2: Scenario-Based Practice

---

## Scenario 1: Service not starting

**Situation:** A web application service called `myapp` failed to start after a server reboot. What commands would you run to diagnose?

**Step 1:** Check service status  
**Command:** `systemctl status myapp`  
**Why:** See if it’s failed, inactive, or in another state, and read the one-line hint.

**Step 2:** Check recent logs for the unit  
**Command:** `journalctl -u myapp -n 50 --no-pager`  
**Why:** Error messages here usually say why it failed (config, port, permission, dependency).

**Step 3:** Check if it’s enabled on boot  
**Command:** `systemctl is-enabled myapp`  
**Why:** If disabled, it won’t start on reboot even after we fix the failure.

**Step 4:** Check for failed units and dependencies  
**Command:** `systemctl list-dependencies myapp` and `systemctl --failed`  
**Why:** A failed dependency (e.g. network, disk mount) can prevent myapp from starting.

---

## Scenario 2: High CPU usage

**Situation:** The application server is slow; you SSH in. What commands find which process is using high CPU?

**Step 1:** See live CPU usage  
**Command:** `top` (then press `q` to quit), or `htop`  
**Why:** Shows processes sorted by CPU; identify the PID and command name.

**Step 2:** Get top CPU consumers (non-interactive)  
**Command:** `ps aux --sort=-%cpu | head -10`  
**Why:** Snapshot of top 10 processes by CPU for logs or scripts.

**Step 3:** Note the PID  
**Why:** Use this PID for `strace`, `kill`, or further inspection (e.g. `ls -l /proc/<PID>/exe`).

---

## Scenario 3: Finding service logs

**Situation:** “Where are the logs for the `docker` service?” The service is managed by systemd.

**Step 1:** Confirm the unit name and status  
**Command:** `systemctl status docker`  
**Why:** Confirms the unit name (e.g. `docker.service`) and that it’s a systemd service.

**Step 2:** Last N lines of logs  
**Command:** `journalctl -u docker -n 50 --no-pager`  
**Why:** systemd services log to journald; `-u docker` filters by unit, `-n 50` limits lines.

**Step 3:** Follow logs in real time  
**Command:** `journalctl -u docker -f`  
**Why:** Like `tail -f` for the unit; useful while reproducing an issue.

---

## Scenario 4: File permissions issue

**Situation:** A script at `/home/user/backup.sh` does not run. Running `./backup.sh` gives “Permission denied”.

**Step 1:** Check current permissions  
**Command:** `ls -l /home/user/backup.sh`  
**Look for:** Something like `-rw-r--r--` (no `x`). So the file is not executable.

**Step 2:** Add execute permission  
**Command:** `chmod +x /home/user/backup.sh`  
**Why:** Adds execute (`x`) for owner (and often group/others depending on umask).

**Step 3:** Verify permissions  
**Command:** `ls -l /home/user/backup.sh`  
**Look for:** `-rwxr-xr-x` (or similar) so that `x` is present.

**Step 4:** Run the script  
**Command:** `./backup.sh` (or `bash /home/user/backup.sh`)  
**Why:** Confirms it runs; if it still fails, the error is likely inside the script (e.g. path, interpreter).

---

_Run the Part 1 commands and scenario steps on your system and adjust paths/service names (e.g. `myapp` → your real service) as needed._
