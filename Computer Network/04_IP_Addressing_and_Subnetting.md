# 🏷️ IP Addressing & Subnetting

> An **IP Address** is a unique numerical identifier assigned to every device on a network, allowing it to be located and communicated with. **Subnetting** divides a large network into smaller, manageable sub-networks.

---

## 🎯 Why Do We Need IP Addressing?

🔴 Every device needs a unique identity to send/receive data correctly

🔴 Without addressing, routers wouldn't know where to deliver packets

🔴 A single large network is wasteful and hard to manage — subnetting solves this

### Example

```text
Task                                    | Needs
-------------------------------------------------------
Identify a device on the Internet        | A unique IP Address
Send data to the correct device           | IP Address + Routing
Split a company network into departments   | Subnetting
Conserve limited IP address space           | Subnetting / CIDR
```

---

# 🧠 The IP Addressing Roadmap

```text
IP Addressing
 ↓
 ├── IPv4              → 32-bit address (most common)
 ├── IPv6               → 128-bit address (newer, larger space)
 ├── Address Classes     → A, B, C, D, E
 ├── Subnetting           → Dividing networks using subnet masks
 └── CIDR                  → Modern flexible addressing notation
```

---

# 1️⃣ IPv4 Addressing

### Definition

> IPv4 is a **32-bit** address, written as four decimal numbers (0-255) separated by dots, called **dotted decimal notation**.

### Structure

```text
192.168.1.1
 ↓     ↓   ↓  ↓
8bits 8bits 8bits 8bits   = 32 bits total
```

### Two Parts of an IP Address

```text
Network ID   → Identifies which network the device belongs to
Host ID       → Identifies the specific device within that network
```

### Interview Shortcut

> **IPv4 = 32-bit address, 4 octets, dotted decimal notation (e.g., 192.168.1.1).**

---

# 2️⃣ IP Address Classes

### Definition

> IPv4 addresses are divided into 5 classes (A–E) based on their range, primarily used to determine the default split between Network ID and Host ID.

| Class | Range | Default Subnet Mask | Used For |
| ------- | ------- | ----------------------- | ---------- |
| **A** | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 | Large networks |
| **B** | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 | Medium networks |
| **C** | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 | Small networks |
| **D** | 224.0.0.0 – 239.255.255.255 | N/A | Multicasting |
| **E** | 240.0.0.0 – 255.255.255.255 | N/A | Experimental/Reserved |

### Interview Shortcut

> **A = huge networks (few networks, many hosts). C = small networks (many networks, few hosts).**

---

# 3️⃣ Private vs Public IP Addresses

### Private IP Ranges (Not routable on the Internet)

```text
10.0.0.0     – 10.255.255.255    (Class A)
172.16.0.0   – 172.31.255.255    (Class B)
192.168.0.0  – 192.168.255.255   (Class C)
```

### Public IP

> Globally unique, routable directly over the Internet, assigned by ISPs.

### Interview Shortcut

> **Private IP = used inside LANs. Public IP = used to communicate over the Internet.**

---

# 4️⃣ Subnet Mask

### Definition

> A Subnet Mask is a 32-bit number that separates the **Network ID** portion from the **Host ID** portion of an IP address.

### Example

```text
IP Address:    192.168.1.10
Subnet Mask:   255.255.255.0

→ Network Portion: 192.168.1.0
→ Host Portion:    .10
```

### How Masking Works (Binary)

```text
IP:      11000000.10101000.00000001.00001010
Mask:    11111111.11111111.11111111.00000000
         └────── Network ──────┘└── Host ──┘
```

### Interview Shortcut

> **Subnet Mask = a filter that splits IP into Network + Host portions.**

---

# 5️⃣ Subnetting

### Definition

> Subnetting is the process of dividing a single large network into multiple smaller sub-networks (subnets), improving organization, security, and efficient address utilization.

### Why Subnet?

```text
✔ Reduces network congestion (smaller broadcast domains)
✔ Improves security (isolate departments/teams)
✔ Efficient use of IP address space
✔ Easier network management and troubleshooting
```

### Example — Subnetting a /24 Network

```text
Original Network: 192.168.1.0/24   (256 addresses, mask 255.255.255.0)

Need: 4 subnets

Borrow 2 bits from Host portion → /26 (255.255.255.192)

Resulting Subnets:
192.168.1.0/26    → Hosts: 192.168.1.1   - 192.168.1.62
192.168.1.64/26   → Hosts: 192.168.1.65  - 192.168.1.126
192.168.1.128/26  → Hosts: 192.168.1.129 - 192.168.1.190
192.168.1.192/26  → Hosts: 192.168.1.193 - 192.168.1.254
```

> Each subnet has 64 addresses total, but only 62 usable host addresses (first = network address, last = broadcast address).

### Interview Shortcut

> **Subnetting = borrowing bits from the Host portion to create more, smaller networks.**

---

# 6️⃣ CIDR (Classless Inter-Domain Routing)

### Definition

> CIDR is a modern addressing method that replaces the rigid Class A/B/C system with a flexible **"/n" notation**, indicating exactly how many bits are used for the network portion.

### Example

```text
192.168.1.0/24
              ↑
        24 bits = Network portion
        8 bits  = Host portion (32 - 24)

→ Total Hosts = 2^8 = 256
→ Usable Hosts = 256 - 2 = 254  (minus network & broadcast address)
```

### Common CIDR Notations

| CIDR | Subnet Mask | Total Addresses | Usable Hosts |
| ------ | --------------- | ------------------- | --------------- |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /30 | 255.255.255.252 | 4 | 2 |

### Interview Shortcut

> **CIDR = "/n" tells you how many bits are Network bits. Formula: Usable Hosts = 2^(32-n) - 2.**

---

# 7️⃣ IPv6 (Brief Overview)

### Definition

> IPv6 is a **128-bit** address format designed to replace IPv4 due to address exhaustion, written in 8 groups of hexadecimal numbers separated by colons.

### Example

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

### Why IPv6?

```text
✔ IPv4 has only ~4.3 billion addresses — already exhausted
✔ IPv6 offers 2^128 addresses — virtually unlimited
✔ Built-in security (IPsec) and simplified header
```

### Interview Shortcut

> **IPv6 = 128-bit, hexadecimal, colon-separated. Solves IPv4 address exhaustion.**

---

# ⚖️ IPv4 vs IPv6

| Feature | IPv4 | IPv6 |
| -------- | ------ | ------ |
| Address Length | 32-bit | 128-bit |
| Notation | Dotted decimal (192.168.1.1) | Hexadecimal, colon-separated |
| Total Addresses | ~4.3 billion | ~340 undecillion |
| Header | Complex | Simplified |
| Security | Optional (IPsec separate) | Built-in (IPsec mandatory) |

---

# 📌 Quick Revision

| Concept | Core Idea |
| --------- | ----------- |
| IPv4 | 32-bit address, dotted decimal |
| Address Classes | A (large) → E (experimental) |
| Private IP | Used within LANs, not routable on Internet |
| Subnet Mask | Splits IP into Network + Host portions |
| Subnetting | Dividing one network into smaller subnets |
| CIDR | "/n" notation showing network bit count |
| IPv6 | 128-bit address, solves IPv4 exhaustion |

---

# 🎤 Viva Questions

### What is an IP Address?

> A unique numerical identifier assigned to a device on a network, used to locate and communicate with that device.

### What is the difference between a Network ID and a Host ID?

> The Network ID identifies which network a device belongs to, while the Host ID identifies the specific device within that network.

### What is a Subnet Mask used for?

> It separates the Network ID portion from the Host ID portion of an IP address, helping define the size and boundary of a subnet.

### What is Subnetting and why is it used?

> Subnetting divides a large network into smaller sub-networks to improve security, reduce congestion, and make efficient use of IP address space.

### What is CIDR notation?

> A flexible notation (e.g., /24) that specifies exactly how many bits of an IP address are used for the network portion, replacing the rigid Class A/B/C system.

### How do you calculate the number of usable hosts in a subnet?

> Usable Hosts = 2^(32 - n) - 2, where n is the CIDR prefix length, and we subtract 2 for the network address and broadcast address.

### What is the difference between a public and a private IP address?

> A public IP is globally unique and routable on the Internet, while a private IP is used only within a local network and is not routable externally.

### Why was IPv6 introduced?

> Because IPv4's ~4.3 billion addresses were running out due to the explosive growth of Internet-connected devices, and IPv6 provides a vastly larger address space.

### Give an example of a Class C private IP address range.

> 192.168.0.0 to 192.168.255.255 is the Class C private IP address range.

### What do the first and last addresses in a subnet represent?

> The first address in a subnet is the Network Address (identifies the subnet itself), and the last address is the Broadcast Address (used to send data to all devices in that subnet) — neither is assignable to a host.

---

## 🏆 One-Line Summary

```text
IPv4            → 32-bit, dotted decimal (192.168.1.1)

Address Classes  → A (large) to E (experimental)

Subnet Mask        → Splits IP into Network + Host portions

Subnetting           → Divides one network into smaller subnets

CIDR                   → "/n" notation, Usable Hosts = 2^(32-n) - 2

IPv6                     → 128-bit, hexadecimal, solves address exhaustion
```

---



## References

1. Behrouz A. Forouzan — *TCP/IP Protocol Suite*, 4th Edition, McGraw-Hill
2. Andrew S. Tanenbaum — *Computer Networks*, 5th Edition, Pearson
3. James F. Kurose, Keith W. Ross — *Computer Networking: A Top-Down Approach*, 7th Edition, Pearson

---

<div align="center">

### ⭐ Star this repository if it helped you learn Computer Networks!

</div>
