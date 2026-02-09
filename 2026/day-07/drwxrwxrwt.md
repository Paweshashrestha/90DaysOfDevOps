`drwxrwxrwt` is a **Linux file permission string** you see when you run commands like:

```bash
ls -ld /tmp
```

Example output:

```
drwxrwxrwt 10 root root 4096 Mar 10 10:00 /tmp
```

Let’s break **`drwxrwxrwt`** step-by-step in a very simple way.

---

# 1️⃣ First Character → File Type

```
d
```

Meaning **directory**

Common ones:

| Symbol | Meaning       |
| ------ | ------------- |
| `-`    | normal file   |
| `d`    | directory     |
| `l`    | symbolic link |

So here:

```
d = directory
```

---

# 2️⃣ Next 9 Characters → Permissions

They are divided into **3 groups**:

```
rwx | rwx | rwt
```

| Group  | Who                    |
| ------ | ---------------------- |
| first  | Owner                  |
| second | Group                  |
| third  | Others (everyone else) |

---

# 3️⃣ Permission Letters

| Letter | Meaning | Number |
| ------ | ------- | ------ |
| r      | read    | 4      |
| w      | write   | 2      |
| x      | execute | 1      |

So:

```
rwx = 4+2+1 = 7
```

---

# 4️⃣ Owner Permissions

```
rwx
```

Owner can:

✔ read
✔ write
✔ execute

Example:

```
owner can create files
owner can delete files
owner can open files
```

---

# 5️⃣ Group Permissions

```
rwx
```

Group members also can:

✔ read
✔ write
✔ execute

---

# 6️⃣ Others Permissions

```
rwt
```

Here something special appears:

```
t
```

This is called **Sticky Bit**

---

# 7️⃣ What is Sticky Bit (`t`)?

Sticky bit means:

👉 **Users can only delete their own files**

Even if everyone has write permission.

---

### Example: `/tmp` directory

```
drwxrwxrwt
```

Everyone can create files in `/tmp`.

But:

| User  | File  | Can Delete? |
| ----- | ----- | ----------- |
| user1 | file1 | YES         |
| user2 | file1 | ❌ NO        |

Only:

* file owner
* directory owner
* root

can delete it.

---

# 8️⃣ Why Sticky Bit is Important

Without sticky bit:

Anyone could delete anyone’s files.

Example danger:

```
/tmp
```

If sticky bit removed:

```
drwxrwxrwx
```

Then:

```
user2 could delete user1 file
```

That would be **very unsafe**.

---

# 9️⃣ Real System Example

Check `/tmp`:

```bash
ls -ld /tmp
```

Output:

```
drwxrwxrwt 10 root root 4096 Mar 10 10:00 /tmp
```

Meaning:

* directory
* everyone can write
* sticky bit protects files

---

# 🔟 Quick Visual Summary

```
drwxrwxrwt
│││ │││ ││└─ sticky bit (others execute)
│││ │││ │└── others write
│││ │││ └─── others read
│││ ││└───── group execute
│││ │└────── group write
│││ └─────── group read
││└───────── owner execute
│└────────── owner write
└─────────── owner read
```

---

# 1️⃣1️⃣ How to Set Sticky Bit

Command:

```bash
chmod +t directory
```

Example:

```bash
chmod +t shared_folder
```

or numeric:

```
chmod 1777 folder
```

Explanation:

```
1 = sticky bit
777 = rwx for everyone
```

---

# 1️⃣2️⃣ Interview One-Line Answer

**`drwxrwxrwt` means:**

> A directory where owner, group, and others have full permissions, but the sticky bit ensures users can only delete their own files.

---

