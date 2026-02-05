

# Linux Commands Cheat Sheet – Day 03

## 1️⃣ Process Management

| Command                      | Usage                                  | Example                           | Memory Hook                                  |
| ---------------------------- | -------------------------------------- | --------------------------------- | -------------------------------------------- |
| ps aux                       | List all processes with user info      | `ps aux \| grep nginx`            | "a = all, u = user, x = hidden processes 👻" |
| ps -ef                       | List processes with PPID & full format | `ps -ef \| head -20`              | "f = full info 📝"                           |
| top                          | Live CPU/memory usage                  | `top`                             | "top = top performers 🏆"                    |
| htop                         | Interactive, colorful top              | `htop`                            | "h = human-friendly 🌈"                      |
| kill -15 <PID>               | Gracefully stop a process              | `kill -15 1234`                   | "15 = please stop 🙏"                        |
| kill -9 <PID>                | Force kill a process                   | `kill -9 1234`                    | "9 = nuclear option 💥"                      |
| pkill <name>                 | Kill process by name                   | `pkill firefox`                   | "pkill = process name killer 🔫"             |
| killall <name>               | Kill all processes with same name      | `killall chrome`                  | "killall = everyone down ⚡"                  |
| renice <priority> -p <PID>   | Change process priority                | `renice -n 10 -p 1234`            | "renice = rewind priority ⏪"                 |
| nice -n <priority> <command> | Start process with priority            | `nice -n 19 ./heavy-script.sh`    | "nice = start politely 🙏"                   |
| jobs                         | Show shell background jobs             | `jobs`                            | "jobs = my running tasks 📝"                 |
| fg <job>                     | Bring job to foreground                | `fg %1`                           | "fg = front stage 🎭"                        |
| bg <job>                     | Send job to background                 | `bg %1`                           | "bg = back stage 🎬"                         |
| pstree                       | Show process tree                      | `pstree -p`                       | "tree = family hierarchy 🌳"                 |
| pgrep <name>                 | Find process by name                   | `pgrep -l nginx`                  | "grep = process search 🔍"                   |

---

## 2️⃣ File System / Files

| Command                    | Usage                        | Example                              | Memory Hook                          |
| -------------------------- | ---------------------------- | ------------------------------------ | ------------------------------------ |
| ls -l                      | List files with details      | `ls -l /var/log`                     | "l = long format 📄"                 |
| ls -a                      | Show hidden files            | `ls -la ~`                           | "a = all files 👻"                   |
| ls -i                      | Show inode numbers           | `ls -i`                              | "i = index numbers 🏷️"              |
| ls -d */                   | List only directories        | `ls -d */`                           | "d = directories 📁"                 |
| cd <path>                  | Change directory             | `cd /etc/nginx`                      | "cd = change directory 🚪"           |
| pwd                        | Print current directory      | `pwd`                                | "pwd = present working directory 📍" |
| mkdir <dir>                | Create new directory         | `mkdir -p project/src`               | "mkdir = make dir 🏗️"               |
| rm -r <dir>                | Remove directory recursively | `rm -rf old_backup/`                 | "r = remove recursively 🗑️"         |
| cp <src> <dest>            | Copy file/directory          | `cp -r src/ backup/`                 | "cp = copy paste 📋"                 |
| mv <src> <dest>            | Move or rename               | `mv old.txt new.txt`                 | "mv = move/rename 🚚"                |
| touch <file>               | Create empty file            | `touch newfile.txt`                  | "touch = tap & create ✨"             |
| find <dir> -name <pattern> | Search files by name         | `find . -name "*.log"`               | "find = detective 🔎"                |
| stat <file>                | File info & metadata         | `stat /etc/passwd`                   | "stat = status check 🧾"             |
| du -sh <dir>               | Directory size               | `du -sh /var`                        | "du = disk usage 📏"                 |
| tree                       | Show folder tree             | `tree -L 2`                          | "tree = visual hierarchy 🌲"         |

---

## 3️⃣ Networking / Troubleshooting

| Command            | Usage                         | Example                              | Memory Hook                            |
| ------------------ | ----------------------------- | ------------------------------------ | -------------------------------------- |
| ping <host>        | Test connectivity             | `ping -c 4 google.com`               | "ping = heartbeat 🩺"                  |
| ip addr            | Show IP addresses             | `ip addr show`                       | "ip = network identity 🌐"             |
| dig <domain>       | DNS lookup                    | `dig example.com`                    | "dig = DNS detective 🔍"               |
| curl <url>         | Fetch HTTP content            | `curl -I https://example.com`        | "curl = client URL 🌊"                 |
| netstat -tulpn     | Listening ports & connections | `netstat -tulpn`                     | "net = network stats 📊"               |
| traceroute <host>  | Trace path to host            | `traceroute 8.8.8.8`                 | "trace = path traveler 🛣️"            |
| ss -tulw           | Modern netstat alternative    | `ss -tulpn`                          | "ss = socket stats 🔌"                 |
| ifconfig           | Show network interfaces       | `ifconfig eth0`                      | "if = interface config 🌉"             |
| wget <url>         | Download files                | `wget https://example.com/file.zip`  | "wget = web get 🖥️"                   |
| nslookup <domain>  | DNS lookup                    | `nslookup github.com`                | "nslookup = name server detective 🕵️" |
| tcpdump -i <iface> | Capture network traffic       | `tcpdump -i eth0 -c 10`              | "tcpdump = packet spy 📦"              |
| mtr <host>         | Ping + traceroute combo       | `mtr google.com`                     | "mtr = network path tracker 🚀"        |
| arp -a             | Show ARP table                | `arp -a`                             | "arp = MAC/IP mapping 🗺️"             |
| route -n           | Show routing table            | `route -n`                           | "route = traffic map 🛣️"              |
| nmap <host>        | Scan open ports               | `nmap -sT localhost`                 | "nmap = network scanner 🔍"            |

---

## 4️⃣ Logs / System Info

| Command           | Usage                     | Example                      | Memory Hook                         |
| ----------------- | ------------------------- | ---------------------------- | ----------------------------------- |
| journalctl -xe    | View system logs          | `journalctl -u nginx -n 50`  | "journal = detective logs 🔎"       |
| dmesg             | Kernel & boot messages    | `dmesg \| tail -20`         | "dmesg = device messages ⚡"         |
| uptime            | System load & uptime      | `uptime`                     | "uptime = how long alive ⏱️"        |
| free -h           | Memory usage              | `free -h`                    | "free = RAM free/used 🧠"           |
| df -h             | Disk usage                | `df -h`                      | "df = disk free 💽"                 |
| top               | Live CPU/memory usage     | `top`                        | "top = top performers 🏆"           |
| htop              | Interactive, colorful top | `htop`                       | "htop = human-friendly 🌈"          |
| ps aux            | List processes            | `ps aux`                     | "ps = process snapshot 📝"          |
| vmstat            | Virtual memory stats      | `vmstat 1 5`                 | "vmstat = RAM & CPU stats 📊"       |
| iostat            | Disk IO stats             | `iostat -x 2`                | "iostat = IO monitor 💿"            |
| lsof              | List open files           | `lsof -i :80`                | "lsof = file handle check 🔑"       |
| cat /proc/cpuinfo | CPU info                  | `cat /proc/cpuinfo`          | "cpuinfo = brain details 🧠"        |
| cat /proc/meminfo | Memory info               | `cat /proc/meminfo`          | "meminfo = memory map 🗺️"          |
| hostnamectl       | Show hostname & OS info   | `hostnamectl`                | "hostnamectl = system identity 🏷️" |
| who               | Logged-in users           | `who`                        | "who = check audience 👀"           |

---

## 5️⃣ Text, Search & Filters

| Command               | Usage                        | Example                            | Memory Hook                          |
| --------------------- | ---------------------------- | ---------------------------------- | ------------------------------------ |
| grep <pattern> <file> | Search text in files         | `grep -r "error" /var/log`         | "grep = get regular expression 🔍"   |
| grep -E                | Extended regex               | `grep -E "err\|warn" app.log`      | "E = extended pattern 📐"            |
| sed 's/old/new/'      | Stream editor, find & replace | `sed 's/foo/bar/g' file.txt`      | "sed = stream editor ✏️"             |
| awk '{print $1}'      | Column/field processing     | `awk '{print $1,$3}' file.txt`     | "awk = text columns 📊"              |
| head -n <N>           | First N lines                | `head -20 access.log`               | "head = top of file 📄"               |
| tail -n <N>           | Last N lines                 | `tail -f /var/log/syslog`          | "tail = end + follow 📜"             |
| less <file>           | Pager, scroll large files    | `less +F app.log`                  | "less = view without loading 📖"     |
| wc -l                 | Line/word/char count         | `wc -l file.txt`                   | "wc = word count 📏"                 |
| sort                  | Sort lines                   | `sort -n numbers.txt`              | "sort = order lines 📑"              |
| uniq                  | Remove adjacent duplicates   | `sort file \| uniq`                | "uniq = unique lines only 🎯"        |
| cut -d -f             | Cut columns by delimiter     | `cut -d: -f1 /etc/passwd`          | "cut = slice columns ✂️"             |
| tr 'a' 'b'            | Translate/delete chars       | `echo "hello" \| tr 'a-z' 'A-Z'`   | "tr = translate 🔄"                  |
| xargs                 | Run command with stdin args  | `find . -name "*.txt" \| xargs rm` | "xargs = args from pipe 🔗"         |

---

## 6️⃣ Permissions & Ownership

| Command              | Usage                        | Example                        | Memory Hook                          |
| -------------------- | ---------------------------- | ------------------------------ | ------------------------------------ |
| chmod <mode> <file>  | Change file permissions      | `chmod 755 script.sh`          | "chmod = change mode 🔐"             |
| chmod +x <file>      | Make executable              | `chmod +x install.sh`          | "x = executable ▶️"                  |
| chown user:group     | Change owner and group       | `chown www-data:www-data file` | "chown = change owner 👤"            |
| chgrp <group> <file> | Change group only            | `chgrp dev project/`           | "chgrp = change group 👥"            |
| umask                | Default permission mask      | `umask 022`                     | "umask = default mask 🎭"            |
| id                   | User/group IDs               | `id`                           | "id = who am I 🪪"                   |
| whoami               | Current username             | `whoami`                       | "whoami = my name 🏷️"                |
| groups               | User's groups                | `groups`                       | "groups = my memberships 👥"         |

---

## 7️⃣ Archives & Compression

| Command                | Usage                        | Example                          | Memory Hook                          |
| ---------------------- | ---------------------------- | -------------------------------- | ------------------------------------ |
| tar -cvf archive.tar   | Create tar archive           | `tar -cvf backup.tar dir/`        | "c = create, v = verbose 📦"         |
| tar -xvf archive.tar   | Extract tar archive          | `tar -xvf backup.tar`            | "x = extract 📤"                     |
| tar -tzf archive.tar   | List contents                | `tar -tzf backup.tar.gz`         | "t = list, z = gzip 📋"              |
| gzip <file>            | Compress (replaces file)     | `gzip big.log`                   | "gzip = shrink 📉"                   |
| gunzip <file>.gz       | Decompress                   | `gunzip file.gz`                 | "gunzip = unshrink 📈"               |
| zip -r out.zip <dir>   | Create zip archive           | `zip -r project.zip project/`    | "zip = zip archive 🗜️"              |
| unzip archive.zip      | Extract zip                  | `unzip archive.zip -d dest/`    | "unzip = unpack zip 📂"              |

---
