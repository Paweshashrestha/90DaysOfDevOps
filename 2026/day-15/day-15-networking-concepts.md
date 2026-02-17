---

# Day 15 – Networking Concepts: DNS, IP, Subnets & Ports

Networking is **one of the most important skills for DevOps**.

Why?

Because DevOps engineers constantly deal with:

* Servers
* Containers
* Kubernetes clusters
* Databases
* APIs
* Load balancers
* Cloud networking

All of these rely on **DNS, IP addresses, subnets, and ports**.

Think of networking like **a city**:

| Concept    | Real World Example         |
| ---------- | -------------------------- |
| IP Address | House address              |
| DNS        | Phonebook (name → address) |
| Subnet     | Neighborhood               |
| Port       | Door number in a building  |

---

# 1. DNS – How Names Become IPs

## What happens when you type `google.com` in a browser?

Step-by-step:

1. You type **google.com**
2. Your computer asks **DNS server** → "What is the IP of google.com?"
3. DNS server responds with something like:

```
142.250.183.14
```

4. Your browser connects to that **IP address**
5. The website loads.

So:

```
google.com  → DNS → 142.250.183.14
```

DNS basically translates **human-friendly names → machine IP addresses**.

---

## Real DevOps Example

Imagine your app:

```
api.mycompany.com
```

DNS maps it to:

```
34.120.56.21
```

If the server changes, you **only update DNS**, not the application.

---

# DNS Record Types

These are different **types of DNS entries**.

## A Record

Maps **domain → IPv4 address**

Example:

```
google.com → 142.250.183.14
```

Example DNS entry:

```
google.com  A  142.250.183.14
```

---

## AAAA Record

Same as A record but for **IPv6**.

Example:

```
google.com → 2404:6800:4009:80b::200e
```

---

## CNAME Record

Alias for another domain.

Example:

```
www.myapp.com → myapp.com
```

DNS entry:

```
www  CNAME  myapp.com
```

Meaning:

```
www.myapp.com → myapp.com → IP
```

---

## MX Record

Used for **email servers**.

Example:

```
gmail.com → mail servers
```

Entry:

```
gmail.com MX mail.google.com
```

This tells email where to go.

---

## NS Record

Specifies **which DNS server is responsible for the domain**.

Example:

```
google.com NS ns1.google.com
```

---

# Run DNS Query

Command:

```bash
dig google.com
```

Example output:

```
;; ANSWER SECTION:
google.com.   300   IN   A   142.250.183.14
```

Breakdown:

| Field          | Meaning     |
| -------------- | ----------- |
| google.com     | domain      |
| 300            | TTL         |
| A              | record type |
| 142.250.183.14 | IP          |

---

## What is TTL?

TTL = **Time To Live**

Example:

```
TTL = 300 seconds
```

Means:

DNS cache expires in **5 minutes**.

---

# 2. IP Addressing

## What is IPv4?

An **IP address uniquely identifies a device on a network.**

Example:

```
192.168.1.10
```

IPv4 uses:

```
32 bits
```

Split into **4 sections**

```
192.168.1.10
```

Each section:

```
0 – 255
```

Binary representation:

```
11000000.10101000.00000001.00001010
```

---

## Example Network

Devices in your home:

| Device | IP           |
| ------ | ------------ |
| Laptop | 192.168.1.10 |
| Phone  | 192.168.1.12 |
| Router | 192.168.1.1  |

---

# Public vs Private IP

## Public IP

Accessible **from the internet**.

Example:

```
8.8.8.8
```

Used by:

- websites
- servers
- cloud machines

Example:

Your AWS server:

```
54.201.110.15
```

---

## Private IP

Used **inside internal networks**.

Example:

```
192.168.1.10
```

These cannot be accessed from the internet directly.

---

# Private IP Ranges

These ranges are **reserved for private networks**.

| Range                         | Example      |
| ----------------------------- | ------------ |
| 10.0.0.0 – 10.255.255.255     | 10.1.2.3     |
| 172.16.0.0 – 172.31.255.255   | 172.20.10.5  |
| 192.168.0.0 – 192.168.255.255 | 192.168.1.15 |

Example:

```
Home WiFi → 192.168.x.x
AWS VPC → 10.x.x.x
```

---

# Check Your IP

Command:

```
ip addr show
```

Example output:

```
inet 192.168.1.45/24
```

Meaning:

```
IP = 192.168.1.45
Subnet = /24
```

---

# 3. CIDR & Subnetting

CIDR = **Classless Inter-Domain Routing**

Example:

```
192.168.1.0/24
```

---

## What does /24 mean?

It means:

```
24 bits for network
8 bits for hosts
```

Subnet mask:

```
255.255.255.0
```

---

## Example Network

```
192.168.1.0/24
```

Range:

```
192.168.1.0 – 192.168.1.255
```

Total IPs:

```
256
```

But:

- first IP = network address
- last IP = broadcast

Usable hosts:

```
254
```

---

# Usable Host Formula

```
2^(32 - CIDR) - 2
```

---

# CIDR Table

| CIDR | Subnet Mask     | Total IPs | Usable Hosts |
| ---- | --------------- | --------- | ------------ |
| /24  | 255.255.255.0   | 256       | 254          |
| /16  | 255.255.0.0     | 65,536    | 65,534       |
| /28  | 255.255.255.240 | 16        | 14           |

---

# Why Do We Subnet?

Subnetting helps:

### 1. Network organization

Example:

```
10.0.0.0/16
```

Split into:

```
10.0.1.0/24 → backend
10.0.2.0/24 → frontend
10.0.3.0/24 → database
```

---

### 2. Security

Database subnet can block internet.

---

### 3. Traffic control

Different subnets → different rules.

---

## DevOps Example

AWS VPC:

```
10.0.0.0/16
```

Subnets:

```
10.0.1.0/24 → web servers
10.0.2.0/24 → application
10.0.3.0/24 → database
```

---

# 4. Ports – Doors to Services

IP identifies the **machine**.

Ports identify the **application**.

Example:

```
192.168.1.10:80
```

Meaning:

```
IP = server
Port = web service
```

Think:

```
IP = building
Port = room number
```

---

# Common Ports

| Port  | Service |
| ----- | ------- |
| 22    | SSH     |
| 80    | HTTP    |
| 443   | HTTPS   |
| 53    | DNS     |
| 3306  | MySQL   |
| 6379  | Redis   |
| 27017 | MongoDB |

---

# Example

SSH into server:

```
ssh user@192.168.1.20
```

Uses:

```
port 22
```

---

Example web request:

```
https://google.com
```

Uses:

```
port 443
```

---

# Check Open Ports

Command:

```
ss -tulpn
```

Example output:

```
tcp LISTEN 0 128 0.0.0.0:22 users:(("sshd"))
tcp LISTEN 0 128 0.0.0.0:3306 users:(("mysqld"))
```

Meaning:

| Port | Service |
| ---- | ------- |
| 22   | SSH     |
| 3306 | MySQL   |

---

# 5. Putting It All Together

## Scenario 1

```
curl http://myapp.com:8080
```

What happens?

1. DNS resolves:

```
myapp.com → IP
```

2. Curl connects to:

```
IP:8080
```

3. Port **8080** is used by web application.

Concepts involved:

- DNS
- IP
- Port
- HTTP protocol

---

# Scenario 2

Your app cannot connect to:

```
10.0.1.50:3306
```

First things to check:

### 1. Is server reachable?

```
ping 10.0.1.50
```

---

### 2. Is MySQL running?

```
systemctl status mysql
```

---

### 3. Is port open?

```
ss -tulpn | grep 3306
```

---

### 4. Firewall blocking?

```
ufw status
```

or

```
iptables -L
```

---

### 5. Network / subnet rules

Example:

```
Security group blocking port 3306
```

---

# Real DevOps Example (Very Important)

User request flow:

```
User
  ↓
DNS
  ↓
Load Balancer (IP)
  ↓
Web Server (Port 80)
  ↓
Application
  ↓
Database (Port 3306)
```

Every step involves:

- DNS
- IP
- Subnets
- Ports

---

# 3 Key Things I Learned

1. **DNS translates domain names into IP addresses so computers can communicate.**

2. **CIDR and subnetting divide networks into smaller manageable segments for scalability and security.**

3. **Ports allow multiple services to run on the same server by assigning each service a unique door number.**

---

# DevOps Interview Questions

Very common ones:

### What happens when you type a URL in browser?

Explain:

```
DNS → IP → TCP connection → HTTP request → server response
```

---

### Difference between TCP and UDP?

TCP:

```
Reliable
Example: HTTP, SSH
```

UDP:

```
Fast but unreliable
Example: DNS, streaming
```

---

### What is CIDR?

CIDR is a notation that defines **network size using prefix length (e.g., /24).**

---
