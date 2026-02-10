
---

# Day 08 – Cloud Server Setup: Docker, Nginx & Web Deployment

## Commands Used

### Connect to EC2 via SSH

```bash
ssh -i your-key.pem ubuntu@<your-instance-ip>
```

This command connects to the AWS EC2 instance using the SSH private key.

---

### Update System

```bash
sudo apt update
sudo apt upgrade -y
```

This updates the package list and upgrades installed packages.

---

### Install Docker

```bash
sudo apt install docker.io -y
```

Verify Docker installation:

```bash
docker --version
```

Start Docker service:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

### Install Nginx

```bash
sudo apt install nginx -y
```

Verify Nginx status:

```bash
sudo systemctl status nginx
```

Check if Nginx is running:

```bash
sudo systemctl start nginx
```

---

### Check Nginx Logs

Navigate to the log directory:

```bash
cd /var/log/nginx
```

View logs:

```bash
sudo tail -f access.log
```

or

```bash
sudo tail -f error.log
```

---

### Save Logs to File

```bash
sudo tail -100 /var/log/nginx/access.log > ~/nginx-logs.txt
```

This extracts the last 100 lines of the access log and saves them to a file.

---

### Download Logs to Local Machine

Run on local machine terminal:

```bash
scp -i your-key.pem ubuntu@<your-instance-ip>:~/nginx-logs.txt .
```

This securely copies the log file from the cloud server to the local machine.

---

# Challenges Faced

### Permission Denied While Saving Logs

When trying to save logs using:

```bash
sudo tail -100 /var/log/nginx/access.log > nginx-logs.txt
```

I received the error:

```
Permission denied
```

### Solution

I saved the file in my home directory instead:

```bash
sudo tail -100 /var/log/nginx/access.log > ~/nginx-logs.txt
```

This worked because the home directory allows write permissions.

---

# What I Learned

* How to launch and connect to a cloud server using SSH.
* Installing and managing services like Nginx on a Linux server.
* Understanding how to configure security groups to allow web traffic.
* Accessing and extracting server logs for debugging.
* Downloading files from a remote server using `scp`.

---

# Why This Matters for DevOps

This exercise is important because it teaches key DevOps skills:

* **Cloud Infrastructure Management** – launching and configuring servers.
* **Remote Server Administration** – managing systems using SSH.
* **Service Deployment** – installing and running web servers like Nginx.
* **Log Monitoring** – accessing and analyzing server logs.
* **Security Management** – configuring firewall rules and security groups.

These skills are used daily by DevOps engineers to manage production environments.

---

