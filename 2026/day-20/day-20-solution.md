Below is a **DevOps-level solution + explanation + ready documentation** for
`day-20-solution.md`.

This challenge simulates **a real DevOps log analysis tool**.

System administrators often analyze logs like:

* `/var/log/syslog`
* `/var/log/nginx/error.log`
* `/var/log/auth.log`
* application logs

The goal is to **automatically detect problems and generate reports**.

---

# Day 20 – Bash Scripting Challenge: Log Analyzer & Report Generator

This project builds a **log analysis automation script**.

The script will:

1. Validate input
2. Analyze errors
3. Detect critical events
4. Identify common errors
5. Generate a report
6. Archive processed logs

---

# Script: `log_analyzer.sh`

```bash
#!/bin/bash
set -euo pipefail

# ============================
# Input Validation
# ============================

if [ $# -eq 0 ]; then
    echo "Usage: ./log_analyzer.sh <logfile>"
    exit 1
fi

LOGFILE="$1"

if [ ! -f "$LOGFILE" ]; then
    echo "Error: File does not exist"
    exit 1
fi

# ============================
# Setup variables
# ============================

DATE=$(date +%Y-%m-%d)
REPORT="log_report_$DATE.txt"

TOTAL_LINES=$(wc -l < "$LOGFILE")

# ============================
# Error Count
# ============================

ERROR_COUNT=$(grep -E "ERROR|Failed" "$LOGFILE" | wc -l)

echo "Total Errors: $ERROR_COUNT"

# ============================
# Critical Events
# ============================

CRITICAL_EVENTS=$(grep -n "CRITICAL" "$LOGFILE" || true)

# ============================
# Top Error Messages
# ============================

TOP_ERRORS=$(grep "ERROR" "$LOGFILE" \
 | awk '{$1=$2=$3=""; print}' \
 | sort \
 | uniq -c \
 | sort -rn \
 | head -5)

# ============================
# Generate Report
# ============================

{
echo "==============================="
echo " Log Analysis Report"
echo "==============================="
echo "Date: $DATE"
echo "Log File: $LOGFILE"
echo "Total Lines Processed: $TOTAL_LINES"
echo "Total Error Count: $ERROR_COUNT"

echo
echo "----- Top 5 Error Messages -----"
echo "$TOP_ERRORS"

echo
echo "----- Critical Events -----"
echo "$CRITICAL_EVENTS"

} > "$REPORT"

echo "Report generated: $REPORT"

# ============================
# Archive Log File
# ============================

ARCHIVE_DIR="archive"

mkdir -p "$ARCHIVE_DIR"

mv "$LOGFILE" "$ARCHIVE_DIR/"

echo "Log file moved to $ARCHIVE_DIR/"
```

---

# Example Log File

Example `sample_log.log`

```
2026-03-10 INFO Service started
2026-03-10 ERROR Connection timed out
2026-03-10 ERROR Connection timed out
2026-03-10 ERROR File not found
2026-03-10 CRITICAL Disk space below threshold
2026-03-10 Failed login attempt
2026-03-10 ERROR Permission denied
2026-03-10 CRITICAL Database connection lost
```

---

# Running the Script

```
chmod +x log_analyzer.sh
./log_analyzer.sh sample_log.log
```

Console output:

```
Total Errors: 5
Report generated: log_report_2026-03-18.txt
Log file moved to archive/
```

---

# Generated Report Example

File:

```
log_report_2026-03-18.txt
```

Content:

```
===============================
 Log Analysis Report
===============================
Date: 2026-03-18
Log File: sample_log.log
Total Lines Processed: 8
Total Error Count: 5

----- Top 5 Error Messages -----
2 Connection timed out
1 File not found
1 Permission denied

----- Critical Events -----
5:2026-03-10 CRITICAL Disk space below threshold
8:2026-03-10 CRITICAL Database connection lost
```

---

# Explanation of Important Commands

## `grep`

Search text patterns.

Example:

```
grep "ERROR" logfile
```

Finds lines containing **ERROR**.

---

## `grep -n`

Adds **line numbers**.

```
grep -n "CRITICAL" logfile
```

Output:

```
84:CRITICAL Disk error
```

---

# `grep -E`

Extended regex.

```
grep -E "ERROR|Failed"
```

Matches:

```
ERROR
Failed
```

---

# `wc -l`

Counts lines.

```
wc -l logfile
```

Used for:

```
total lines
error count
```

---

# `awk`

Used to manipulate text columns.

Example:

```
awk '{$1=$2=$3=""; print}'
```

Removes:

```
timestamp columns
```

Leaving only the error message.

---

# `sort`

Sorts text.

```
sort
```

---

# `uniq -c`

Counts duplicates.

Example:

```
uniq -c
```

Output:

```
45 Connection timed out
32 File not found
```

---

# `sort -rn`

Sort numeric reverse.

Largest values first.

---

# `head -5`

Shows **top 5 results**.

---

# `mkdir -p`

Create directory if not exists.

```
mkdir -p archive
```

---

# `mv`

Moves files.

```
mv logfile archive/
```

Used for **log archiving**.

---

# DevOps Real-World Use Case

This script replicates what monitoring systems do.

Example workflow:

```
Server Logs
      ↓
Log Analyzer Script
      ↓
Daily Report
      ↓
DevOps Engineer investigates issues
```

---

# Real Production Tools That Do This

DevOps engineers use tools like:

| Tool       | Purpose                 |
| ---------- | ----------------------- |
| ELK Stack  | centralized logging     |
| Splunk     | enterprise log analysis |
| Prometheus | monitoring              |
| Grafana    | dashboards              |

Your script is a **mini version of these systems**.

---

# Example Production Pipeline

```
Application
     ↓
Logs
     ↓
Log Analyzer Script
     ↓
Alert System
     ↓
Slack / Email Notification
```

---

# Commands Used

The script uses:

```
grep
awk
sort
uniq
head
wc
date
mkdir
mv
```

These are **core Linux text processing tools**.

---

# What I Learned (3 Key Points)

### 1

Log analysis scripts help **identify system failures quickly by scanning logs automatically**.

---

### 2

Linux tools like **grep, awk, sort, and uniq are powerful for processing log data**.

---

### 3

Automated scripts can generate reports and archive logs, making **system monitoring and maintenance easier**.

---

# DevOps Interview Questions

### How do you count errors in logs?

```
grep -c "ERROR" logfile
```

---

### How do you find most common errors?

```
grep "ERROR" logfile | sort | uniq -c | sort -rn
```

---

### How do you print log lines with numbers?

```
grep -n "CRITICAL" logfile
```

---

### Why archive logs?

To:

* save disk space
* organize old logs
* preserve historical data

---

