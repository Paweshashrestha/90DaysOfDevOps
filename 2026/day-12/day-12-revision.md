# Day 12 – Breather & Revision (Days 01–11)

Revision notes and self-check. Fill in "What I observed" after re-running commands on your system.

---

## 1. What I Reviewed

### Mindset & plan (Day 01)
- Revisited Day 01 learning plan and goals.
- **Tweaks (if any):** _Add any changes to your 90-day plan or focus areas._

### Processes & services (Days 04–05)
Reran 2 commands and noted what I saw:

| Command | What I observed |
|--------|-----------------|
| `ps aux \| head -10` | _e.g. Listed first 10 processes; system load looks normal._ |
| `systemctl status sshd` (or another service) | _e.g. active (running); enabled._ |
| `journalctl -u sshd -n 20 --no-pager` | _e.g. Last 20 lines; no errors._ |

### File skills (Days 06–11)
Practiced 3 quick ops:

| Op | Command I ran | What I observed |
|----|----------------|-----------------|
| Append to file | `echo "revision note" >> /tmp/rev.txt` | _File updated._ |
| Permissions | `chmod 644 /tmp/rev.txt` or similar | _Permissions set._ |
| List / copy | `ls -l /tmp/rev.txt` or `cp /tmp/rev.txt /tmp/rev.bak` | _Verified._ |

### Cheat sheet refresh (Day 03)
- Skimmed Day 03 Linux commands.
- **5 commands I’d reach for first in an incident:**
  1. `systemctl status <service>` – is the service up?
  2. `journalctl -u <service> -n 50` – what do logs say?
  3. `df -h` – is disk full?
  4. `free -h` – is memory OK?
  5. `ps aux \| grep <name>` – is the process running?

### User/group sanity (Day 09 or 11)
- _e.g. Created a test user or changed ownership of one file._
- **Command:** `sudo chown user:group /path/to/file` or `id testuser`
- **Verify:** `ls -l` or `id <user>` – _noted result._

---

## 2. Mini Self-Check (short answers)

**1) Which 3 commands save you the most time right now, and why?**
- `systemctl status <service>` – quick health check without opening logs.
- `journalctl -u <service> -n 50` – recent errors in one place.
- `ls -l` / `chmod` / `chown` – fixing permission issues fast.

**2) How do you check if a service is healthy? List the exact 2–3 commands you’d run first.**
- `systemctl status <service>` – active/running vs failed.
- `journalctl -u <service> -n 50 --no-pager` – last 50 log lines.
- `ss -tulpn \| grep <port>` or `curl -I <url>` – is it listening/responding?

**3) How do you safely change ownership and permissions without breaking access? Give one example command.**
- Check current state first: `ls -l <file>`. Then change group (keeps owner): `sudo chgrp developers /opt/dev-project`. Or add execute only: `chmod +x script.sh` (adds +x, doesn’t remove r/w). Example: `sudo chown www-data:www-data /var/www/file` after confirming owner/group.

**4) What will you focus on improving in the next 3 days?**
- _e.g. Get faster with `journalctl` flags, practice `chmod` numeric modes, or run one full troubleshooting drill from Day 05._

---

## 3. Key takeaways (bullets)

- Revisiting Day 01 plan keeps goals aligned.
- Process + service + log commands (ps, systemctl, journalctl) are the first line of defense.
- File ops (redirection, chmod, chown) and user/group basics from Day 09 stay in muscle memory with a quick rerun.
- Cheat sheet top 5: status, logs, disk, memory, process list.

---

*Optional: Add a screenshot of one command rerun below.*
