Day 11 is **about WHO owns files** in Linux.
Permissions (Day 10) answer **“what can be done?”**
Ownership answers **“who controls it?”**.

Think of Linux files like **a house with a landlord and a family**.

| Concept | Real-life analogy         |
| ------- | ------------------------- |
| owner   | landlord of the house     |
| group   | family members            |
| others  | everyone else in the city |

So every file in Linux has **3 identities**:

```
owner | group | others
```

---

# 1️⃣ Understanding Ownership

Run:

```bash
ls -l
```

Example output:

```
-rw-r--r-- 1 pawesha developers 45 Mar 12 devops-file.txt
```

Break it into parts:

| Part              | Meaning          |
| ----------------- | ---------------- |
| `-rw-r--r--`      | permissions      |
| `1`               | number of links  |
| `pawesha`         | **owner (user)** |
| `developers`      | **group**        |
| `45`              | file size        |
| `Mar 12`          | date             |
| `devops-file.txt` | filename         |

Important part for Day 11:

```
owner     group
pawesha   developers
```

---

## Difference Between Owner and Group

### Owner

The **main user who controls the file**.

Owner usually has **full control**.

Example:

```
tokyo → owner of access-codes.txt
```

So tokyo can:

```
edit file
delete file
change permissions
```

---

### Group

A **collection of users** who share access.

Example group:

```
developers
```

Members:

```
tokyo
berlin
nairobi
```

If file group is:

```
developers
```

Then **everyone in that group can access depending on permissions**.

Example:

```
-rw-rw-r--
tokyo developers code.py
```

Meaning:

| Who              | Access       |
| ---------------- | ------------ |
| tokyo            | read + write |
| developers group | read + write |
| others           | read only    |

---

# 2️⃣ Task 2 — Basic `chown`

`chown` means:

```
change owner
```

---

## Step 1 — Create File

```
touch devops-file.txt
```

Check owner:

```
ls -l devops-file.txt
```

Example:

```
-rw-r--r-- 1 pawesha pawesha 0 Mar 12 devops-file.txt
```

Owner = pawesha

Group = pawesha

---

## Step 2 — Change Owner to tokyo

```
sudo chown tokyo devops-file.txt
```

Check again:

```
ls -l devops-file.txt
```

Output:

```
-rw-r--r-- 1 tokyo pawesha 0 Mar 12 devops-file.txt
```

Now:

```
owner = tokyo
group = pawesha
```

---

## Step 3 — Change Owner to berlin

```
sudo chown berlin devops-file.txt
```

Check again:

```
ls -l devops-file.txt
```

Output:

```
-rw-r--r-- 1 berlin pawesha 0 Mar 12 devops-file.txt
```

Owner successfully changed.

---

# 3️⃣ Task 3 — `chgrp`

`chgrp` means:

```
change group
```

---

## Step 1 — Create File

```
touch team-notes.txt
```

Check:

```
ls -l team-notes.txt
```

Example:

```
-rw-r--r-- 1 pawesha pawesha team-notes.txt
```

Group = pawesha

---

## Step 2 — Create Group

```
sudo groupadd heist-team
```

Verify:

```
cat /etc/group | grep heist-team
```

---

## Step 3 — Change Group

```
sudo chgrp heist-team team-notes.txt
```

Verify:

```
ls -l team-notes.txt
```

Example:

```
-rw-r--r-- 1 pawesha heist-team team-notes.txt
```

Now:

```
owner = pawesha
group = heist-team
```

---

# 4️⃣ Change Owner AND Group Together

You can do both in **one command**.

Syntax:

```
sudo chown owner:group file
```

---

Example:

Create file:

```
touch project-config.yaml
```

Change owner + group:

```
sudo chown professor:heist-team project-config.yaml
```

Check:

```
ls -l project-config.yaml
```

Output:

```
-rw-r--r-- 1 professor heist-team project-config.yaml
```

---

# 5️⃣ Directory Ownership

Create directory:

```
mkdir app-logs
```

Change owner + group:

```
sudo chown berlin:heist-team app-logs
```

Check:

```
ls -ld app-logs
```

Output:

```
drwxr-xr-x 2 berlin heist-team app-logs
```

---

# 6️⃣ Recursive Ownership (`-R`)

`-R` means:

```
apply changes to everything inside directory
```

---

Create structure:

```
mkdir -p heist-project/vault
mkdir -p heist-project/plans
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf
```

Create group:

```
sudo groupadd planners
```

Change ownership recursively:

```
sudo chown -R professor:planners heist-project
```

Verify:

```
ls -lR heist-project
```

Now **every file and directory inside** belongs to:

```
owner → professor
group → planners
```

---

# 7️⃣ Practice Challenge Example

Create directory:

```
mkdir bank-heist
```

Create files:

```
touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt
```

---

Change ownership:

```
sudo chown tokyo:vault-team bank-heist/access-codes.txt
sudo chown berlin:tech-team bank-heist/blueprints.pdf
sudo chown nairobi:vault-team bank-heist/escape-plan.txt
```

Verify:

```
ls -l bank-heist
```

Example:

```
-rw-r--r-- tokyo   vault-team access-codes.txt
-rw-r--r-- berlin  tech-team  blueprints.pdf
-rw-r--r-- nairobi vault-team escape-plan.txt
```

---

# 8️⃣ Real DevOps Example

Example production server:

```
/var/www/app
```

Ownership:

```
owner = deploy
group = developers
```

Permissions:

```
drwxrwxr-x
```

Meaning:

| User             | Access       |
| ---------------- | ------------ |
| deploy           | full         |
| developers group | read + write |
| others           | read         |

Developers can update code but **cannot break server configs**.

---

# 9️⃣ Important DevOps Insight

Ownership is used for:

### Application deployments

```
/var/www/app
```

Owned by:

```
deploy user
```

---

### Logs

```
/var/log/nginx
```

Owned by:

```
root
```

Group:

```
adm
```

---

### Docker

Docker socket:

```
/var/run/docker.sock
```

Group:

```
docker
```

Users added to docker group can run docker commands.

---

# 🔟 Common Interview Questions

### Q1

What does `chown` do?

Answer:

```
changes file owner
```

---

### Q2

What does `chgrp` do?

Answer:

```
changes file group
```

---

### Q3

How to change owner and group together?

```
chown user:group file
```

---

### Q4

How to change ownership recursively?

```
chown -R user:group directory
```

---

# Key DevOps Mental Model

Every Linux file has:

```
Owner → main controller
Group → team access
Others → public access
```

Permissions decide **what they can do**.

---

