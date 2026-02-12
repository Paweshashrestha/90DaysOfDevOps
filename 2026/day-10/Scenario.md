
Normal permissions are:

```
rwxrwxrwx
```

But Linux also has **three extra bits**:

| Special Permission | Symbol | Purpose                                   |
| ------------------ | ------ | ----------------------------------------- |
| **setuid**         | `s`    | Run program with owner's privileges       |
| **setgid**         | `s`    | Run with group privileges / inherit group |
| **sticky bit**     | `t`    | Protect files in shared directories       |

Let's understand them **deeply with real-world examples**.

---

# 1️⃣ setuid (Set User ID)

### What it does

When **setuid** is set on an executable file, the program runs with the **file owner's permissions**, not the user running it.

This sounds small but is **very powerful**.

---

### Real Example in Linux

Run:

```bash
ls -l /usr/bin/passwd
```

Typical output:

```
-rwsr-xr-x 1 root root 68248 /usr/bin/passwd
```

Notice:

```
rws
```

Instead of:

```
rwx
```

The **s** means **setuid is enabled**.

---

### Why this is needed

When a normal user runs:

```
passwd
```

They are actually modifying:

```
/etc/shadow
```

But:

```
/etc/shadow
```

is only writable by **root**.

So how can a normal user change their password?

Answer:

```
passwd program runs as root using setuid
```

This is why you can run:

```
passwd
```

without root.

---

### How to set setuid

Example:

Create script:

```
vim test.sh
```

Content:

```
echo "Hello"
```

Make executable:

```
chmod +x test.sh
```

Set setuid:

```
chmod u+s test.sh
```

Check:

```
ls -l test.sh
```

Output:

```
-rwsr-xr-x
```

The **s** replaces the owner's **x**.

---

### Numeric form

```
chmod 4755 file
```

Breakdown:

| Digit | Meaning |
| ----- | ------- |
| 4     | setuid  |
| 7     | owner   |
| 5     | group   |
| 5     | others  |

---

# 2️⃣ setgid (Set Group ID)

### What it does

Two main uses:

1️⃣ On **executables** → run with file's group privileges
2️⃣ On **directories** → files inherit the directory's group

The second one is **very important for DevOps shared folders**.

---

### Real DevOps Scenario

Imagine a shared project folder:

```
/opt/dev-project
```

Developers:

```
tokyo
berlin
alice
```

Group:

```
developers
```

If someone creates a file:

```
touch api.py
```

Normally:

```
api.py → owner group = user's primary group
```

This causes **permission conflicts**.

---

### Solution → setgid on directory

Command:

```
chmod g+s /opt/dev-project
```

Check:

```
ls -ld /opt/dev-project
```

Output:

```
drwxrwsr-x
```

Notice:

```
rws
```

Now every file created inside automatically becomes:

```
group = developers
```

This is **very commonly used in DevOps**.

---

### Numeric form

```
chmod 2755 directory
```

| Digit | Meaning |
| ----- | ------- |
| 2     | setgid  |
| 7     | owner   |
| 5     | group   |
| 5     | others  |

---

# 3️⃣ Sticky Bit

### Purpose

Protect files in **shared directories**.

Without sticky bit:

Anyone with write access could delete other users' files.

---

### Example shared directory

```
/tmp
```

Run:

```
ls -ld /tmp
```

Output:

```
drwxrwxrwt
```

Notice:

```
t
```

This is **sticky bit**.

---

### What sticky bit does

Even if directory is writable:

```
user1 cannot delete user2's files
```

Only these can delete:

| Who             | Allowed |
| --------------- | ------- |
| File owner      | ✔       |
| Directory owner | ✔       |
| root            | ✔       |

Everyone else → ❌

---

### Example

Without sticky bit:

```
user1 creates file
user2 deletes it
```

With sticky bit:

```
Permission denied
```

---

### Set sticky bit

```
chmod +t directory
```

Example:

```
mkdir shared
chmod 1777 shared
```

Check:

```
ls -ld shared
```

Output:

```
drwxrwxrwt
```

---

### Numeric form

```
chmod 1777 directory
```

| Digit | Meaning    |
| ----- | ---------- |
| 1     | sticky bit |
| 7     | owner      |
| 7     | group      |
| 7     | others     |

---

# Quick Comparison

| Feature | Symbol | Used On                 | Purpose                      |
| ------- | ------ | ----------------------- | ---------------------------- |
| setuid  | s      | executables             | run as owner                 |
| setgid  | s      | executables/directories | run as group / inherit group |
| sticky  | t      | directories             | protect files                |

---

# How DevOps Engineers Use Them

### setuid

Programs that require **temporary root access**

Example:

```
passwd
ping
sudo
```

---

### setgid

Shared project folders:

```
/opt/project
/var/www
/shared/code
```

Ensures team files keep same group.

---

### sticky bit

Public shared folders:

```
/tmp
/shared/uploads
```

Prevents users deleting each other's files.

---

# Interview Question

A common DevOps interview question:

**Question**

Why does `/tmp` have `drwxrwxrwt`?

Answer:

Because the **sticky bit prevents users from deleting files they do not own**.

---

# Visual Memory Trick

```
s → super power (setuid/setgid)
t → protected trash (sticky bit like /tmp)
```

---
