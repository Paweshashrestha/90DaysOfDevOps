---

# Day 19 – Shell Scripting Project: Log Rotation, Backup & Crontab

This project combines everything learned in **Days 16–18**:

* functions
* loops
* arguments
* error handling
* strict mode
* file handling
* automation

---

# Task 1 – Log Rotation Script

Servers generate **huge logs**:

Examples:

```
/var/log/nginx/access.log
/var/log/syslog
/var/log/app.log
```

If not managed:

- disk becomes full
- server crashes

So DevOps engineers implement **log rotation**.

---

# Script: `log_rotate.sh`

```bash
#!/bin/bash
set -euo pipefail

LOG_DIR=$1

if [ ! -d "$LOG_DIR" ]; then
    echo "Error: Directory does not exist"
    exit 1
fi

compressed_count=0
deleted_count=0

# Compress logs older than 7 days
for file in $(find "$LOG_DIR" -name "*.log" -mtime +7); do
    gzip "$file"
    ((compressed_count++))
done

# Delete compressed logs older than 30 days
for file in $(find "$LOG_DIR" -name "*.gz" -mtime +30); do
    rm "$file"
    ((deleted_count++))
done

echo "Compressed files: $compressed_count"
echo "Deleted files: $deleted_count"
```

---

## Run Script

```
./log_rotate.sh /var/log/myapp
```

Example output:

```
Compressed files: 3
Deleted files: 1
```

---

# Important Command: `find`

Example:

```
find /var/log -name "*.log" -mtime +7
```

Meaning:

| Part           | Meaning           |
| -------------- | ----------------- |
| find           | search files      |
| -name "\*.log" | log files         |
| -mtime +7      | older than 7 days |

---

# Production Example

Real servers often use **logrotate** tool.

But DevOps engineers also write custom scripts for:

- application logs
- container logs
- monitoring logs

---

# Task 2 – Server Backup Script

Backups protect against:

- server crashes
- accidental deletion
- ransomware
- database corruption

A DevOps engineer always ensures **automated backups**.

---

# Script: `backup.sh`

```bash
#!/bin/bash
set -euo pipefail

SOURCE=$1
DEST=$2

if [ ! -d "$SOURCE" ]; then
    echo "Source directory does not exist"
    exit 1
fi

if [ ! -d "$DEST" ]; then
    echo "Destination directory does not exist"
    exit 1
fi

TIMESTAMP=$(date +%Y-%m-%d)

ARCHIVE="$DEST/backup-$TIMESTAMP.tar.gz"

tar -czf "$ARCHIVE" "$SOURCE"

if [ -f "$ARCHIVE" ]; then
    SIZE=$(du -h "$ARCHIVE" | cut -f1)
    echo "Backup created: $ARCHIVE"
    echo "Size: $SIZE"
else
    echo "Backup failed"
    exit 1
fi

# Delete backups older than 14 days
find "$DEST" -name "backup-*.tar.gz" -mtime +14 -delete
```

---

## Run Script

```
./backup.sh /home/app /backups
```

Example output:

```
Backup created: /backups/backup-2026-03-18.tar.gz
Size: 120M
```

---

# Important Command: `tar`

Archive command:

```
tar -czf archive.tar.gz folder
```

Meaning:

| Flag | Meaning          |
| ---- | ---------------- |
| -c   | create           |
| -z   | gzip compression |
| -f   | filename         |

Example:

```
tar -czf backup.tar.gz /var/www
```

---

# Task 3 – Crontab

Cron schedules scripts automatically.

Example:

```
run backup every night
```

Instead of manually running scripts.

---

# View Cron Jobs

```
crontab -l
```

Example output:

```
0 2 * * * /scripts/log_rotate.sh
```

---

# Cron Syntax

```
* * * * * command
```

Meaning:

```
minute hour day month weekday
```

---

# Cron Examples

### Run log rotation daily at 2 AM

```
0 2 * * * /scripts/log_rotate.sh /var/log/myapp
```

Explanation:

| Field   | Value |
| ------- | ----- |
| minute  | 0     |
| hour    | 2     |
| day     | \*    |
| month   | \*    |
| weekday | \*    |

---

### Run backup every Sunday at 3 AM

```
0 3 * * 0 /scripts/backup.sh /home/app /backups
```

`0` = Sunday.

---

### Run health check every 5 minutes

```
*/5 * * * * /scripts/health_check.sh
```

`*/5` = every 5 minutes.

---

# Editing Cron Jobs

Open cron editor:

```
crontab -e
```

Add entries.

---

# Task 4 – Maintenance Script

Instead of many cron jobs, DevOps often uses **one master maintenance script**.

---

# Script: `maintenance.sh`

```bash
#!/bin/bash
set -euo pipefail

LOGFILE="/var/log/maintenance.log"

log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> "$LOGFILE"
}

rotate_logs() {
    ./log_rotate.sh /var/log/myapp
}

run_backup() {
    ./backup.sh /home/app /backups
}

main() {
    log "Starting maintenance"

    rotate_logs
    log "Log rotation completed"

    run_backup
    log "Backup completed"

    log "Maintenance finished"
}

main
```

---

# Example Log Output

File:

```
/var/log/maintenance.log
```

Example:

```
2026-03-18 01:00:01 - Starting maintenance
2026-03-18 01:00:03 - Log rotation completed
2026-03-18 01:00:10 - Backup completed
2026-03-18 01:00:10 - Maintenance finished
```

---

# Cron Entry for Maintenance

Run daily at **1 AM**:

```
0 1 * * * /scripts/maintenance.sh
```

---

# Real Production DevOps Workflow

Example server automation:

```
1 AM  → maintenance script
2 AM  → log rotation
3 AM  → database backup
Every 5 min → health checks
```

Cron handles everything.

---

# DevOps Architecture Example

Typical production server maintenance:

```
Server
 ├── logs
 │    ├── rotated
 │    └── compressed
 │
 ├── backups
 │    ├── daily
 │    └── weekly
 │
 └── cron
      └── scheduled scripts
```

---

# What I Learned (3 Key Points)

### 1

Log rotation prevents logs from filling disk space by **compressing and deleting old files**.

---

### 2

Backup scripts ensure **data recovery in case of server failure or data loss**.

---

### 3

Cron automates scripts by running them **at scheduled times without manual intervention**.

---

# DevOps Interview Questions

### What is cron?

Cron is a **Linux scheduler used to run scripts automatically at specific times.**

---

### How do you view cron jobs?

```
crontab -l
```

---

### What does this cron mean?

```
*/5 * * * *
```

Run **every 5 minutes**.

---

### What command creates compressed backups?

```
tar -czf backup.tar.gz folder
```

---

### Why rotate logs?

To:

- save disk space
- maintain performance
- archive logs

---
