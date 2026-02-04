# Linux Architecture, Processes, and systemd – Day 02 Notes

## 1️⃣ Core Components of Linux

- **Kernel**: The core of Linux, interacts with hardware, manages memory, processes, and devices.
- **User Space**: Where applications and commands run; everything outside the kernel.
- **Init/systemd**: First process started by the kernel (`PID 1`); responsible for starting services and managing system boot.

---

## 2️⃣ How Applications Talk to Linux

**Flow of a command/application:**

- **Application**: e.g., your text editor, browser, or script
- **Shell**: Command-line interpreter (bash/zsh) that reads your commands
- **Kernel**: Executes requests, manages resources, communicates with hardware
- **Hardware**: Actual CPU, memory, disks, network

> Every action you do in Linux follows this chain

---

## 3️⃣ Processes in Linux (Deep Dive)

### What is a Process?

- A **process** is a **running instance of a program**.
- Each process has:
  - **PID (Process ID)** – unique identifier
  - **Parent PID (PPID)** – which process created it
  - **State** – running, sleeping, stopped, etc.
  - **Memory, CPU usage, open files**

- Think of a process as a **living program** — it takes resources and executes instructions.

---

### How Processes are Created

1. **`fork()`** – Copy a process
   - Creates a new process by **duplicating the parent process**.
   - Both parent and child continue running **separately**.
   - Example memory layout is copied → child starts with same code/data as parent.
   - **Mnemonic:** “fork = fork in the road” → parent splits into two paths (processes).

2. **`exec()`** – Replace process
   - Replaces the **current process image** with a **new program**.
   - Often used after `fork()` to run a different program in the child process.
   - **Mnemonic:** “exec = execute new program” → forget old memory, start fresh.

> **Example flow:**
>
> ```bash
> Parent process (bash)
> └─fork() → Child process (bash copy)
>     └─exec() → New program (e.g., python script)
> ```

---

## 🧠 Linux Process States (Detailed Table)

| State | Name                     | What It Really Means                                             | Uses CPU? | Common Cause                             | How to See         | Memory Trick                           |
| ----- | ------------------------ | ---------------------------------------------------------------- | --------- | ---------------------------------------- | ------------------ | -------------------------------------- |
| **R** | Running                  | Process is executing on CPU **or ready in run queue**            | ✅ Yes    | Active command, loop, heavy computation  | `ps aux` / `top`   | **R = Racing on CPU** 🏃               |
| **S** | Sleeping (Interruptible) | Waiting for event (I/O, input, network, timer)                   | ❌ No     | Waiting for disk, keyboard, API response | `top`              | **S = Sleeping** 😴                    |
| **D** | Uninterruptible Sleep    | Waiting for hardware (usually disk I/O). Cannot be killed easily | ❌ No     | Disk read/write stuck                    | `ps aux`           | **D = Disk wait** 💽                   |
| **T** | Stopped                  | Paused by signal (`Ctrl+Z`, `kill -STOP`)                        | ❌ No     | Manual pause                             | `ps`               | **T = Temporarily stopped** 🛑         |
| **Z** | Zombie                   | Process finished but parent hasn’t read exit status              | ❌ No     | Parent didn't call `wait()`              | `ps -ef \| grep Z` | **Z = Zombie (dead but not buried)** ☠ |
| **I** | Idle (kernel thread)     | Kernel background thread waiting                                 | ❌ No     | System internal work                     | `top`              | **I = Idle kernel** ⚙                  |
| **X** | Dead                     | Fully terminated process                                         | ❌ No     | Process cleaned up                       | Rarely visible     | **X = eXited** ❌                      |



---

## 🧠 `ps -ef | grep Z` — Find Zombie Processes

### 🔹 `ps -ef`

* `ps` = process status
* `-e` = show **all processes**
* `-f` = full detailed format

👉 Shows complete list of processes in the system.

---

### 🔹 `|`

Pipe symbol
👉 Sends output of `ps -ef` to next command.

---

### 🔹 `grep Z`

* `grep` = search text
* `Z` = zombie process state

👉 Filters only lines where process state is **Z (Zombie)**.

---

## 💀 What is Zombie (Z)?

* Process has finished
* Parent has NOT collected exit status
* Shows as `<defunct>`
* Uses **no CPU**, but occupies process table entry

---

## ✅ Better Version (avoids matching itself)

```bash
ps -ef | grep '[Z]'
```

---

### 🧠 One-Line Memory Trick

> `ps -ef | grep Z` = Show all processes → filter → display only zombies 👻

---



# 🔥 Very Important Clarification (Interview Trap)

### 🔴 R does NOT always mean actively running.

It can also mean:

> “Ready to run, waiting for CPU time slice.”

Linux scheduler manages this.

---

# ⚡ Quick Command Reference Table

| Command            | Purpose                   |
| ------------------ | ------------------------- |
| `ps aux`           | Snapshot of all processes |
| `top`              | Live process monitor      |
| `htop`             | Advanced colorful monitor |
| `kill -STOP PID`   | Pause process             |
| `kill -CONT PID`   | Resume process            |
| `kill -9 PID`      | Force kill process        |
| `ps -ef \| grep Z` | Find zombies              |

---

# 🧠 Ultra-Simple Life Analogy

| State | Human Example                                      |
| ----- | -------------------------------------------------- |
| R     | Working                                            |
| S     | Waiting for food delivery                          |
| D     | Waiting for bank server response (can't interrupt) |
| T     | Teacher said "Pause!"                              |
| Z     | Employee resigned but HR didn't finish paperwork   |

---

- **Mnemonic to remember states:**
  **R**un → actively moving
  **S**leep → resting/waiting
  **T**erminated (stopped) → paused
  **Z**ombie → dead but not buried (parent pending)

---

---

## Useful Daily Commands (with memory hooks)

## **1️⃣ `ps aux` – List processes**

- **`a` → all users’ processes**
  - Shows processes of **all users**, not just your shell.
  - Memory Hook: **“a = all”** → everyone is invited 🎉

- **`u` → user-oriented output**
  - Displays **user, CPU%, memory%, start time**, etc.
  - Memory Hook: **“u = user info”** → who owns this process? 🎫

- **`x` → show processes not attached to any terminal**
  - Shows **daemon/background processes** that don’t have a terminal.
  - Memory Hook: **“x = X-files”** → hidden/ghost processes 👻

> So `ps aux` = **all processes, show user info, include hidden ones**

---

## **2️⃣ `kill` – Terminate a process**

- **`-9` → SIGKILL**
  - Forces the process to terminate immediately.
  - Memory Hook: **“9 = nuclear option 💥”** → can’t be ignored.

- **`-15` → SIGTERM**
  - Politely asks the process to terminate. Default signal.
  - Memory Hook: **“15 = please stop 🙏”** → graceful exit.

---

## **3️⃣ `top` / `htop` – Real-time Monitoring**

- **`top`**
  - No letters to break down; just shows **top CPU/memory consumers**.
  - Memory Hook: “top = top performers 🏆”

- **`htop`**
  - **h** = **human-friendly / colorful interactive view**
  - Memory Hook: “h = happy colorful view 🌈”

---

## **4️⃣ `systemctl` / `journalctl`**

- **`systemctl`** → controls systemd services
  - **ctl = control** → “supervisor control panel” 📝

- **`journalctl`** → views system logs
  - **ctl = control** → “detective logs interface 🔍”

---

## ✅ Quick Memory Tips

| Command    | Letter / Number | Meaning            | Hook                    |
| ---------- | --------------- | ------------------ | ----------------------- |
| `ps aux`   | a               | all users          | everyone invited 🎉     |
|            | u               | user info          | shows process owner 🎫  |
|            | x               | include background | ghosts 👻               |
| `kill -9`  | 9               | SIGKILL            | nuclear option 💥       |
| `kill -15` | 15              | SIGTERM            | polite stop 🙏          |
| `htop`     | h               | human-friendly     | colorful interactive 🌈 |

---

### Visual Memory Trick

```
[ Parent Process ]
       |
     fork()
       |
[ Child Process ]
       |
     exec() → New Program
```

- Process lifecycle: **Created → Running → Sleeping → Stopped/Zombie → Terminated**
- Commands act like your **tools to observe, control, and debug** these processes.

---

## 4️⃣ What systemd Does

- systemd = **modern init system** replacing SysVinit
- Responsibilities:
  - Starts and manages services at boot
  - Handles dependencies between services
  - Monitors running services
  - Provides logging interface (`journalctl`)
- Why it matters:
  - Fast, parallelized boot
  - Automatic service restarts
  - Centralized control of services → easier debugging

---

## 5️⃣ Why This Matters for DevOps

- Most production servers run Linux → knowing internals saves time
- Helps to:
  - Debug crashed services faster
  - Fix CPU/memory issues efficiently
  - Understand logs and service restarts confidently
