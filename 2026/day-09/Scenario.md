

---

# 1️⃣ Multi-Developer Server (Most Common Scenario)

Imagine a company has **one Linux server for development**.

Developers:

* tokyo
* berlin
* john
* alice

They all work on the same project.

Instead of giving everyone **root access**, the DevOps engineer creates a group:

```
developers
```

Users are added to it:

```
tokyo → developers
berlin → developers
alice → developers
```

Then a shared directory is created:

```
/opt/project
```

Permissions:

```
775
```

Meaning:

| User Type | Permission         |
| --------- | ------------------ |
| Owner     | read write execute |
| Group     | read write execute |
| Others    | read execute       |

Now **all developers can collaborate safely**.

Example:

```
tokyo creates: api.py
berlin edits: api.py
alice adds: database.py
```

This is **exactly your dev-project example**.

---

# 2️⃣ Production Server Access Control

In production servers, access is **very strictly controlled**.

Example roles:

| Role       | Group      |
| ---------- | ---------- |
| Developers | developers |
| DevOps     | admins     |
| QA         | testers    |

Example:

```
/var/www/app
```

Permissions:

```
Owner → root
Group → developers
```

So developers can **deploy code** but cannot break the system.

Only admins can:

```
restart services
change configs
install packages
```

Example commands admins run:

```
sudo systemctl restart nginx
sudo apt install docker
```

---

# 3️⃣ Shared Data / Logs

Example company structure:

```
/var/log/app
```

Developers may need to read logs to debug.

Group created:

```
log-readers
```

Permissions:

```
chmod 750 /var/log/app
```

Meaning:

| Type   | Access |
| ------ | ------ |
| Owner  | full   |
| Group  | read   |
| Others | none   |

Now developers can debug errors without root.

---

# 4️⃣ CI/CD Systems (Very Real DevOps Example)

Example CI/CD user:

```
jenkins
```

Group:

```
deploy
```

Directory:

```
/opt/deployments
```

Members:

```
jenkins
devops
```

When pipeline runs:

```
jenkins deploys code
```

Command:

```
git pull
npm build
restart server
```

But only **deploy group can modify deployment directory**.

---

# 5️⃣ Security (Very Important)

Instead of giving **root access**, companies do:

```
sudo access
groups
permissions
```

Example:

```
admins group → full access
developers → limited
interns → read-only
```

This prevents accidents.

Example disaster:

A developer accidentally runs:

```
rm -rf /
```

If permissions are correct → **damage is prevented**.

---

# 6️⃣ Cloud Servers (AWS / GCP / Azure)

When you launch a cloud server:

Example:

```
Ubuntu EC2 instance
```

DevOps team creates users:

```
backend-dev
frontend-dev
qa
devops
```

Instead of everyone logging as:

```
ubuntu
```

This improves:

* security
* logging
* accountability

---

# 7️⃣ Large Teams (100+ Engineers)

Large companies manage access using groups.

Example:

```
backend-team
frontend-team
data-team
devops-team
```

Directories:

```
/opt/backend
/opt/frontend
/opt/data
```

Each team has **its own permissions**.

---

# 8️⃣ Kubernetes / Docker Hosts

On container servers:

Example directory:

```
/var/lib/docker
```

Group:

```
docker
```

Users added:

```
sudo usermod -aG docker username
```

Then user can run:

```
docker run
docker build
docker ps
```

Without needing root.

---

# Real DevOps Workflow Example

DevOps engineer sets up server:

```
1. create users
2. create groups
3. assign permissions
4. create shared directories
```

Developers then:

```
ssh tokyo@server
cd /opt/project
git pull
```

Everything works **securely and organized**.

---

# Key DevOps Concept You Just Practiced

Your exercise teaches **three core Linux concepts**:

### 1️⃣ Identity

Users identify **who is accessing system**.

### 2️⃣ Authorization

Groups define **who can access what**.

### 3️⃣ Permissions

chmod controls **what actions are allowed**.

---

# Interview Question From This

A DevOps interviewer may ask:

**Question**

> Multiple developers need to work on the same directory. How would you manage permissions?

Correct answer:

1. Create a group
2. Add developers to group
3. Assign group ownership to directory
4. Set permissions with `chmod`


---

