---
# 🌍 PART 1 — Networking Fundamentals (Expanded)

## 1️⃣ What is a Network?

A **network** is a collection of devices connected so they can communicate.

**Devices in a network:**

* Laptop
* Phone
* Server
* Router

**Communication means:**

* Sending data
* Receiving data

Data travels in small pieces called **packets**.
---

## 2️⃣ What is a Packet?

A **packet** is a small unit of data sent over a network.

### Why packets?

Instead of sending a huge file at once:

- Data is broken into smaller packets
- Each packet travels independently
- Packets are reassembled at the destination

**Advantages:**

- More reliable — if one packet fails, only that packet is resent
- Efficient — network can route different packets via different paths

This concept is called **Packet Switching**.

---

## 3️⃣ How Internet Originally Started

- **1969 – ARPANET**
  Goal: A network should keep working even if some computers or cables fail

**Solution:**

- Use **packet switching**

- Use **decentralized routing**

- **1983 – TCP/IP became official protocol**

> TCP/IP is the language of the internet.
> Internet = network of networks using TCP/IP.

---

## 4️⃣ Types of Networks: LAN, MAN, WAN

### 🏠 LAN (Local Area Network)

- Covers a **small area** like a home or office
- Devices are connected via **WiFi or Ethernet**
- Typically uses **private IP addresses**

#### Private IP addresses

Private IPs are **only for internal use** and **cannot directly access the internet**.
They are translated to public IPs using **NAT (Network Address Translation)**.

**Private IP Ranges (RFC 1918):**

| IP Range                      | CIDR Notation  | Notes                                                 |
| ----------------------------- | -------------- | ----------------------------------------------------- |
| 10.0.0.0 – 10.255.255.255     | 10.0.0.0/8     | ~16 million addresses — large networks                |
| 172.16.0.0 – 172.31.255.255   | 172.16.0.0/12  | ~1 million addresses — medium networks                |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | ~65,000 addresses — common in homes and small offices |

**Example (India Home LAN):**
_Home WiFi:_ Router = 192.168.1.1, Laptop = 192.168.1.10, Phone = 192.168.1.11

---

### 🏙 MAN (Metropolitan Area Network)

- Covers a **city or metropolitan area**
- Larger than LAN but smaller than WAN
- Connects multiple LANs together using **fiber optic backbones**

**India Example (Delhi):**

- Your home LAN: 192.168.1.x
- Neighbor’s LAN: 192.168.2.x
- Both connect to **Delhi MAN**, which is ISP’s fiber network connecting neighborhoods
- Handles traffic inside the city efficiently
- If content (like YouTube cache) is stored locally, MAN can serve it without going abroad

> Think of MAN as the **city’s internal highway** connecting neighborhoods.

---

### 🌎 WAN (Wide Area Network)

- Covers a **large geographical area**, potentially worldwide
- Example: The **Internet** itself
- Connects multiple LANs and MANs

**India Example:**

- You in **Mumbai** want to access a server in **New York, USA**:
  1. Laptop → Home Router → Mumbai MAN → ISP node
  2. ISP routes packet to **national backbone** → submarine cable → US
  3. Server responds → back through same path → router → laptop

**Key Points:**

- WAN uses **fiber optic cables, satellites, microwave links**
- Protocols like **TCP/IP, MPLS, BGP** manage routing between networks
- Backbone is extremely fast, but last mile may be slower

> Think of WAN as **international highways**, MAN as **city roads**, LAN as **house streets**.

---

### Hierarchy

LAN → MAN → WAN

**Comparison Table:**

| Feature  | LAN                   | MAN                     | WAN                                    |
| -------- | --------------------- | ----------------------- | -------------------------------------- |
| Coverage | Home / office         | City / metro area       | Country / worldwide                    |
| Speed    | High (1 Gbps+)        | Very high (10–100 Gbps) | Very high backbone, variable last mile |
| Cost     | Low                   | Medium                  | High                                   |
| Example  | Home WiFi 192.168.x.x | Delhi ISP network       | Internet, India → USA                  |

---

## ✅ Summary

- **Network:** Devices connected to communicate
- **Packet:** Small piece of data, travels independently
- **LAN:** Small area, private IPs, home/office
- **MAN:** City-wide, ISP internal network
- **WAN:** Global network, the Internet

---
