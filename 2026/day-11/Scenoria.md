

---

# 1️⃣ Web Server Deployment (Very Common)

Imagine a company website running on **Nginx**.

Website files are stored in:

```
/var/www/myapp
```

If you check ownership:

```bash
ls -l /var/www
```

You might see:

```
drwxr-xr-x root root myapp
```

But **Nginx runs as user `www-data`**.

If ownership is wrong, the server cannot read files.

### Fix

```bash
sudo chown -R www-data:www-data /var/www/myapp
```

Now the web server can read the files.

### Real Problem DevOps Faces

Error in browser:

```
403 Forbidden
```

Cause:

```
wrong ownership or permissions
```

---

# 2️⃣ Dev Team Shared Project Folder

A team of developers works on the same server.

Users:

```
tokyo
berlin
nairobi
```

Group:

```
developers
```

Project folder:

```
/opt/project
```

Ownership:

```bash
sudo chown -R deploy:developers /opt/project
```

Meaning:

| Role             | Access      |
| ---------------- | ----------- |
| deploy           | main owner  |
| developers group | team access |

Now all developers can collaborate.

---

# 3️⃣ Log Files Management

Applications generate logs:

```
/var/log/app.log
```

Ownership might be:

```
root root
```

But the application runs as:

```
appuser
```

So the app cannot write logs.

### Fix

```bash
sudo chown appuser:appuser /var/log/app.log
```

Now logging works.

---

# 4️⃣ CI/CD Pipeline (Very Real DevOps Case)

Imagine a CI server running **Jenkins**.

Build artifacts are stored in:

```
/opt/builds
```

But the directory is owned by root:

```
root root
```

Jenkins cannot write files.

### Fix

```bash
sudo chown -R jenkins:jenkins /opt/builds
```

Now pipelines can store build files.

---

# 5️⃣ Docker Permission Problem

If you install **Docker**, the socket file is:

```
/var/run/docker.sock
```

Ownership:

```
root docker
```

To allow developers to run docker commands:

```bash
sudo usermod -aG docker tokyo
```

Now tokyo can run:

```
docker run
docker build
```

without root.

---

# 6️⃣ File Upload Server

Imagine a file upload system.

Uploads stored in:

```
/data/uploads
```

Ownership must match the application user:

```bash
sudo chown -R uploaduser:uploads /data/uploads
```

Otherwise users see upload errors.

---

# Typical DevOps Interview Questions

### Question 1

How do you change the owner of a file?

Answer:

```bash
sudo chown user filename
```

---

### Question 2

How do you change both owner and group?

Answer:

```bash
sudo chown user:group filename
```

---

### Question 3

How do you change ownership of a directory and all its files?

Answer:

```bash
sudo chown -R user:group directory
```

---

### Question 4

What is the difference between `chown` and `chgrp`?

| Command | Purpose      |
| ------- | ------------ |
| chown   | change owner |
| chgrp   | change group |

---

### Question 5 (Common)

A web server shows **403 Forbidden**. What could be wrong?

Possible answer:

```
file ownership incorrect
or permissions incorrect
```

Example fix:

```bash
sudo chown -R www-data:www-data /var/www/html
```

---

# Mini Scenario Question (Practice)

Server directory:

```
/var/www/app
```

Ownership:

```
root root
```

Web server user:

```
www-data
```

Problem:

```
Website shows 403 error
```

Question:

What command fixes this?

Answer:

```bash
sudo chown -R www-data:www-data /var/www/app
```

---

# Another DevOps Scenario

Developers need to modify project files:

```
/opt/project
```

Owner:

```
root
```

Group:

```
developers
```

Question:

How to allow developers to edit files?

Answer:

```bash
sudo chown -R root:developers /opt/project
```

---

# Key DevOps Lesson

Ownership answers:

```
Who controls the file?
```

Permissions answer:

```
What actions are allowed?
```

Together they control **Linux security**.

---
