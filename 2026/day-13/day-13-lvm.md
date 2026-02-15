# Linux Volume Management (LVM) — Deep Explanation

## 1. What is Storage in Linux?

In Linux, **storage** refers to any place where data is kept. Examples include:

- Hard disk (HDD)
- Solid-state drive (SSD)
- USB drive
- Virtual disk file

Linux represents these as **devices inside `/dev`**.

Example devices:

```

/dev/sda
/dev/sdb
/dev/loop0

```

These are called **block devices**.

### Block Device

A **block device** stores data in **fixed-size blocks** (usually 512 bytes or 4KB).

Examples:

- Hard disk
- SSD
- USB drive

To list block devices on your system, run:

```bash
lsblk
```

Explanation of command:

- `ls` → list
- `blk` → block devices

# 2. The Problem Without LVM

Normally disk structure looks like:

```
Disk
 ├── Partition 1
 ├── Partition 2
 └── Partition 3
```

Example:

```
/dev/sda1
/dev/sda2
/dev/sda3
```

Problems:

- Hard to resize
- Hard to merge disks
- Space cannot be shared easily
- Must plan size beforehand

Example problem:

You created:

```
/home = 10GB
```

Later you need:

```
/home = 20GB
```

Without LVM:
❌ Hard to expand
❌ May require reinstall

---

# 3. What is LVM?

**LVM = Logical Volume Manager**

It is a **storage virtualization system**.

Meaning:

It **abstracts physical disks into flexible logical storage**.

Think of it like **RAM virtualization but for disks**.

---

### Real-world analogy

Imagine water storage:

Without LVM:

```
Tank A = 100L
Tank B = 100L
Tank C = 100L
```

You must use each tank separately.

With LVM:

```
All tanks → One big water pool
```

Then you create **custom pipes** from that pool.

Those pipes = **Logical Volumes**

---

# 4. LVM Architecture

LVM has **3 main layers**:

```
Disk / Device
      ↓
Physical Volume (PV)
      ↓
Volume Group (VG)
      ↓
Logical Volume (LV)
      ↓
Filesystem
      ↓
Mount Point
```

Let's understand **each word deeply**.

---

# 5. Physical Volume (PV)

### Command

```
pvcreate
```

### Meaning

A **Physical Volume** is a **disk prepared for LVM**.

Example device:

```
/dev/sdb
```

When we run:

```bash
pvcreate /dev/sdb
```

We are saying:

> Convert this disk into an LVM storage unit.

---

### What happens internally?

LVM writes **metadata** on the disk.

This metadata contains:

- LVM UUID
- size
- allocation info

Now the disk becomes:

```
Physical Volume
```

Check it:

```bash
pvs
```

Meaning:

```
pvs = physical volumes show
```

---

# 6. Volume Group (VG)

### Command

```
vgcreate
```

### Meaning

A **Volume Group** is a **pool of storage created from one or more physical volumes**.

Example:

```
PV1 = 10GB
PV2 = 10GB
PV3 = 10GB
```

Create VG:

```
vgcreate devops-vg PV1 PV2 PV3
```

Now:

```
devops-vg = 30GB storage pool
```

So:

```
VG = storage pool
```

Check it:

```bash
vgs
```

Meaning:

```
volume groups show
```

---

# 7. Logical Volume (LV)

### Command

```
lvcreate
```

### Meaning

Logical Volume = **virtual partition created from VG**

Example:

```
Volume Group = 30GB
```

We can create:

```
app-data = 500MB
logs = 5GB
backup = 10GB
```

Each one is a **Logical Volume**.

Example command:

```bash
lvcreate -L 500M -n app-data devops-vg
```

Breakdown:

| Part        | Meaning               |
| ----------- | --------------------- |
| `lvcreate`  | create logical volume |
| `-L`        | size                  |
| `500M`      | 500 megabytes         |
| `-n`        | name                  |
| `app-data`  | LV name               |
| `devops-vg` | volume group          |

Check:

```bash
lvs
```

Meaning:

```
logical volumes show
```

---

# 8. Filesystem

After creating LV, it is **empty raw storage**.

You must create a **filesystem**.

### Filesystem

Filesystem organizes how data is stored.

Examples:

- ext4
- xfs
- btrfs

Most common:

```
ext4
```

Create filesystem:

```bash
mkfs.ext4 /dev/devops-vg/app-data
```

Meaning:

```
mkfs = make filesystem
ext4 = filesystem type
```

---

# 9. Mounting

### Mount

Mount means:

> Attach storage to the Linux directory tree.

Linux has **one big directory structure**.

Example:

```
/
├── home
├── var
├── usr
└── mnt
```

To use storage:

```
mount device → folder
```

Example:

```bash
mkdir -p /mnt/app-data
mount /dev/devops-vg/app-data /mnt/app-data
```

Meaning:

```
This logical volume will appear inside /mnt/app-data
```

Now files stored here go into the LV.

---

# 10. df Command

Command:

```bash
df -h
```

Meaning:

```
df = disk free
-h = human readable
```

Example output:

```
Filesystem      Size Used Avail Mounted on
/dev/sda1       50G  10G   40G /
/dev/devops-vg/app-data 500M 10M 490M /mnt/app-data
```

---

# 11. Extending Storage

One huge advantage of LVM is **resizing volumes easily**.

### Extend Logical Volume

Command:

```bash
lvextend -L +200M /dev/devops-vg/app-data
```

Meaning:

| Part                      | Meaning          |
| ------------------------- | ---------------- |
| `lvextend`                | increase LV size |
| `-L`                      | size             |
| `+200M`                   | add 200MB        |
| `/dev/devops-vg/app-data` | target volume    |

---

### Resize Filesystem

Extending LV is **not enough**.

Filesystem must also grow.

Command:

```bash
resize2fs /dev/devops-vg/app-data
```

Meaning:

```
Resize ext2/ext3/ext4 filesystem
```

---

# 12. Virtual Disk Creation (When No Spare Disk)

If you don't have extra disk.

You simulate one.

### Command

```bash
dd if=/dev/zero of=/tmp/disk1.img bs=1M count=1024
```

Breakdown:

| Part             | Meaning               |
| ---------------- | --------------------- |
| `dd`             | disk duplication tool |
| `if`             | input file            |
| `/dev/zero`      | infinite zeros        |
| `of`             | output file           |
| `/tmp/disk1.img` | disk image            |
| `bs`             | block size            |
| `1M`             | 1 MB                  |
| `count`          | number of blocks      |
| `1024`           | total blocks          |

Result:

```
1MB × 1024 = 1GB disk file
```

---

# 13. Loop Device

Command:

```bash
losetup -fP /tmp/disk1.img
```

Meaning:

Attach disk file as **virtual block device**.

Example result:

```
/dev/loop0
```

Now Linux thinks:

```
loop0 = real disk
```

Check:

```bash
losetup -a
```

---

# 14. Storage Inspection Commands

### lsblk

```
lsblk
```

Shows:

```
NAME   SIZE TYPE
sda     50G disk
sdb     10G disk
loop0    1G loop
```

---

### pvs

Shows physical volumes.

Example:

```
PV        VG        Size
/dev/sdb  devops-vg 10G
```

---

### vgs

Shows volume groups.

Example:

```
VG        PV  LV  Size
devops-vg 1   1   10G
```

---

### lvs

Shows logical volumes.

Example:

```
LV       VG        Size
app-data devops-vg 500M
```

---

# 15. Full Flow of LVM

Real workflow:

```
Create disk
   ↓
pvcreate
   ↓
vgcreate
   ↓
lvcreate
   ↓
mkfs
   ↓
mount
   ↓
use storage
```

---

# 16. Why DevOps Engineers Use LVM

Advantages:

### 1️⃣ Dynamic resizing

You can grow storage **without downtime**.

---

### 2️⃣ Combine multiple disks

```
Disk1 + Disk2 + Disk3
        ↓
   One volume group
```

---

### 3️⃣ Snapshots

Backup state of filesystem.

Example:

```
lvcreate --snapshot
```

---

### 4️⃣ Storage abstraction

Applications don't see real disks.

They see **logical volumes**.

---

# 17. Example Real Production Setup

Server with:

```
Disk1 = 500GB
Disk2 = 500GB
Disk3 = 500GB
```

Create:

```
VG = 1.5TB
```

Then allocate:

```
/var = 300GB
/home = 200GB
/logs = 200GB
/db = 800GB
```

Later:

```
db needs 200GB more
```

Just run:

```
lvextend
resize2fs
```

Done. ⚡

---

# 18. What You Learned (for your markdown)

Example:

**Learning Points**

1️⃣ LVM allows flexible storage management by abstracting physical disks into logical volumes.

2️⃣ Storage can be dynamically extended using `lvextend` and `resize2fs`.

3️⃣ LVM architecture consists of **Physical Volumes → Volume Groups → Logical Volumes**.

---

# 19. What Your Final Markdown Should Contain

```
day-13-lvm.md
```

Structure:

```
# Day 13 - Linux LVM

## Commands Used

lsblk
pvcreate
vgcreate
lvcreate
mkfs.ext4
mount
lvextend
resize2fs

## Screenshots

(paste terminal screenshots)

## What I Learned

1.
2.
3.
```

---
