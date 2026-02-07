# Day 05 – Linux Troubleshooting Runbook

**Target service:** `sshd` (SSH daemon)  
**Purpose:** Quick health snapshot and log trace; repeatable checklist for incidents.

---

## 1. Target service / process

- **Service:** `sshd`
- **Why:** Critical for remote access; if it fails, we lose SSH. Good candidate for a drill.

---

## 2. Snapshot: Environment basics

| # | Command | What I ran / observed |
|---|---------|------------------------|
| 1 | `uname -a` | Kernel version, hostname, architecture. Confirms we’re on the expected machine. |
| 2 | `cat /etc/os-release` | OS name and version. Use `lsb_release -a` if available. |

---

## 3. Snapshot: Filesystem sanity

| # | Command | What I ran / observed |
|---|---------|------------------------|
| 3 | `mkdir -p /tmp/runbook-demo && cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo` | Create a throwaway dir, copy a file, list it. Confirms disk is writable and paths work. |
| 4 | `df -h` | Disk space per filesystem. Watch for any partition near 100%. |

---

## 4. Snapshot: CPU & memory

| # | Command | What I ran / observed |
|---|---------|------------------------|
| 5 | `ps -o pid,pcpu,pmem,comm -p $(pgrep -d, sshd) 2>/dev/null || ps aux \| head -5` | CPU and memory per sshd process (or first 5 processes). Low %CPU/%MEM is normal for sshd. |
| 6 | `free -h` | Total/used/free RAM. Check if "available" is very low. |

---

## 5. Snapshot: Disk & I/O

| # | Command | What I ran / observed |
|---|---------|------------------------|
| 7 | `du -sh /var/log` | Size of log directory. Large growth can fill disk. |
| 8 | `df -h /var` | Space on partition containing `/var`. Ensures logs and data have room. |

---

## 6. Snapshot: Network

| # | Command | What I ran / observed |
|---|---------|------------------------|
| 9 | `ss -tulpn \| grep -E ':22|LISTEN'` | Listening ports; confirm port 22 (SSH) is bound. |
| 10 | `curl -I -s -o /dev/null -w "%{http_code}" http://localhost 2>/dev/null || echo "no http"` | Optional: quick HTTP check. For SSH we care more about `ss`. |

---

## 7. Logs reviewed

| # | Command | What I ran / observed |
|---|---------|------------------------|
| 11 | `journalctl -u sshd -n 50 --no-pager` | Last 50 lines for sshd. Look for "Failed", "error", or "Accepted" (successful logins). |
| 12 | `tail -n 50 /var/log/secure` (RHEL/Fedora) or `tail -n 50 /var/log/auth.log` (Debian/Ubuntu) | Auth-related log tail. Complements journalctl. |

---

## 8. Quick findings template

- **Environment:** OS and kernel as expected? Yes / No
- **Disk:** Any filesystem >90% full? Yes / No
- **Memory:** "available" (from `free -h`) acceptable? Yes / No
- **SSH:** Port 22 listening? Yes / No
- **Logs:** Any critical errors in last 50 lines? Yes / No

*(Fill after running the commands on your system.)*

---

## 9. If this worsens (next steps)

1. **Restart strategy:** If sshd is failed or unresponsive, try `systemctl restart sshd`. If you’re already SSH’d in, use a separate console or out-of-band access to avoid locking yourself out.
2. **Increase log verbosity:** Temporarily set higher log level (e.g. in `/etc/ssh/sshd_config`: `LogLevel DEBUG`), restart sshd, reproduce the issue, then revert and restart.
3. **Collect deeper evidence:** Use `strace -p <sshd-PID>` (with care) or `journalctl -u sshd -f` while reproducing; capture to a file for later analysis. If the box is unstable, take a memory/disk snapshot or core dump if policy allows.

---

*Run the commands on your machine and paste real outputs into this runbook so it reflects your environment.*
