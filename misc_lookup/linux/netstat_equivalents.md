
# `netstat` to Modern Commands (`ss` / `ip`) Cheat Sheet

### 1. Active Connections & Sockets (`ss`)

| Task / Option | Deprecated `netstat` | Modern Equivalent |
| :--- | :--- | :--- |
| **All Sockets** | `netstat -a` | **`ss -a`** |
| **Listening Sockets Only** | `netstat -l` | **`ss -l`** |
| **TCP Connections** | `netstat -t` | **`ss -t`** |
| **UDP Connections** | `netstat -u` | **`ss -u`** |
| **Numeric Addresses (No DNS resolve)** | `netstat -n` | **`ss -n`** |
| **Show Process ID / Program Name** | `netstat -p` | **`ss -p`** |
| **Extended Stats & Timers** | `netstat -e -o` | **`ss -e -o`** |
| **Internal Socket Info (RTT, cwnd, retrans)** | *N/A* | **`ss -i`** or **`ss -ti`** |

#### Common Combinations:
* **`ss -tulpn`** -> Show all listening TCP/UDP sockets with process names and numeric ports *(replaces `netstat -tulpn`)*.
* **`ss -ta`** -> Show all active and listening TCP connections.

---

### 2. Interface Statistics & Link Status (`ip link`)

| Task / Option | Deprecated `netstat` | Modern Equivalent |
| :--- | :--- | :--- |
| **List Interfaces & Link Status** | `netstat -i` | **`ip -s link`** |
| **Continuous Interface Watch** | `netstat -i 1` | **`watch ip -s link`** |

---

### 3. Routing Table (`ip route`)

| Task / Option | Deprecated `netstat` | Modern Equivalent |
| :--- | :--- | :--- |
| **Display Routing Table** | `netstat -r` | **`ip route`** |
| **Numeric Routing Table** | `netstat -rn` | **`ip route`** *(numeric by default)* |

---

### 4. Protocol Statistics & Summary Counters (`nstat` / `ss`)

| Task / Option | Deprecated `netstat` | Modern Equivalent |
| :--- | :--- | :--- |
| **Network Protocol Summary Stats** | `netstat -s` | **`nstat -az`** or **`ss -s`** |
| **Multicast Group Membership** | `netstat -g` | **`ip maddr`** |

---

### Why `ss` is Faster
`netstat` reads slow `/proc/net/` text files line-by-line, causing major slowdowns on systems with thousands of open connections. **`ss`** talks directly to the kernel via high-performance **netlink sockets**, rendering results almost instantly.