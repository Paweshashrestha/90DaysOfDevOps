```bash
scp -i your-key.pem ubuntu@<your-instance-ip>:~/nginx-logs.txt .
```

This command is used to **copy a file from a remote server (EC2) to your local machine**.

`scp` stands for **Secure Copy Protocol**.
It transfers files **securely over SSH**.

---

# 1️⃣ `scp`

`scp` = **secure copy**

It works like `cp` (copy command) but for **remote servers**.

Example:

```bash
cp file.txt backup.txt
```

copies locally.

But:

```bash
scp file.txt user@server:/path
```

copies to a **remote server**.

---

# 2️⃣ `-i your-key.pem`

`-i` means **identity file**.

In AWS, you connect to EC2 using a **private key**.

Example:

```
your-key.pem
```

This key proves:

> "I am allowed to access this server."

Without this key, AWS will **not allow login**.

So this part tells `scp`:

```
Use this SSH key for authentication
```

---

# 3️⃣ `ubuntu@<your-instance-ip>`

This specifies the **remote server**.

Structure:

```
username@server-ip
```

Example:

```
ubuntu@54.201.22.11
```

Meaning:

| Part           | Meaning         |
| -------------- | --------------- |
| `ubuntu`       | server username |
| `54.201.22.11` | EC2 public IP   |

For different systems:

| OS           | Username   |
| ------------ | ---------- |
| Ubuntu       | `ubuntu`   |
| Amazon Linux | `ec2-user` |
| CentOS       | `centos`   |

---

# 4️⃣ `:`

This colon separates:

```
server location : file path
```

Example:

```
ubuntu@server:/home/ubuntu/file.txt
```

Meaning:

> file on the remote server.

---

# 5️⃣ `~/nginx-logs.txt`

This is the **file path on the server**.

`~` means **home directory**.

For user `ubuntu`:

```
~ = /home/ubuntu
```

So the real path becomes:

```
/home/ubuntu/nginx-logs.txt
```

---

# 6️⃣ `.` (dot)

This is very important.

`.` means:

```
current directory
```

So the file will download to **the folder where your terminal is currently located**.

Example:

If you run the command in:

```
/home/pawesha/Downloads
```

The file will appear there.

---

# 📦 Full Meaning of the Command

```
scp -i your-key.pem ubuntu@<your-instance-ip>:~/nginx-logs.txt .
```

Means:

> Copy the file **nginx-logs.txt from the EC2 server** to **my current local directory**, using the **SSH key for authentication**.

---

# 🔎 Real Example

```bash
scp -i aws-key.pem ubuntu@54.210.15.33:~/nginx-logs.txt .
```

This downloads:

```
/home/ubuntu/nginx-logs.txt
```

from the server to your computer.

---

# 📂 After Download

If you run:

```bash
ls
```

You will see:

```
nginx-logs.txt
```

Now the file exists **on your local machine**.

---

# 🔁 Reverse Command (Upload File)

You can also **upload files to the server**.

Example:

```bash
scp -i key.pem file.txt ubuntu@server-ip:~
```

Meaning:

```
local file → remote server
```

---

# 🧠 Easy Way to Remember

```
scp [key] user@server:file destination
```

Example:

```
scp -i key.pem ubuntu@server:file.txt .
```

Server → Local machine.

---
