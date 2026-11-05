# 🌐 42 NetPractice: Ultimate Guide, Subnetting Tricks & Study Blueprint

A complete reference and study companion for the **42 NetPractice** project (`en.subject.pdf`). This guide covers the **official 42 subject rules & turn-in requirements**, core **Cisco networking fundamentals**, **foolproof subnetting shortcuts** (no manual binary conversion needed), **curated YouTube tutorials (NetworkChuck, Jeremy's IT Lab, etc.)**, and **AI prompts** to turn an LLM into your personal 42 peer-evaluator and drill instructor.

---

## 📑 Table of Contents
1. [📋 42 Subject Requirements & Turn-in Rules](#-42-subject-requirements--turn-in-rules)
2. [🚀 Project Overview & Quick Start](#-project-overview--quick-start)
3. [🧠 Cisco Networking Basics & Core Concepts](#-cisco-networking-basics--core-concepts)
4. [⚡ Subnetting Secrets & Mental Math Shortcuts](#-subnetting-secrets--mental-math-shortcuts)
5. [🎯 NetPractice Golden Rules (The 5 Commandments)](#-netpractice-golden-rules-the-5-commandments)
6. [📺 Curated YouTube Study Resources](#-curated-youtube-study-resources)
7. [🤖 How to Practice with AI (Interactive Prompts)](#-how-to-practice-with-ai-interactive-prompts)
8. [🎓 42 Peer Evaluation & Defense Prep](#-42-peer-evaluation--defense-prep)

---

## 📋 42 Subject Requirements & Turn-in Rules

According to the official 42 project subject (`en.subject.pdf`):

### 1. The Core Assignment
- **10 Training Levels**: You must solve 10 simulated network configuration exercises in the browser interface.
- **Practice Mode vs. Correction Mode**:
  - **Practice Mode**: You **MUST enter your 42 intra login** in the login box before solving levels. The simulation generates address parameters based on your login seed.
  - **Correction Mode**: Leave the login field empty (used by peer evaluators during the defense).
- **Interface Controls**:
  - **Unshaded fields**: Editable fields where you type IP addresses, subnet masks, and routing entries.
  - **Shaded fields**: Fixed fields that cannot be modified.
  - **"Check again"**: Simulates packet transmission and displays green/red status plus error logs at the bottom.
  - **"Get my config"**: **Crucial step!** Once all checks pass, click this button to export and download your configuration file for that level.

### 2. Turn-in & Submission Rules
- **Deliverables**: Exactly **10 configuration files** (one exported file per level, `level1` through `level10`).
- **Placement**: All 10 config files must be placed directly at the **root of your Git repository** (`/home/motaz/NetPractice/`).
- **Personal Seed**: Each file must correspond to your personal 42 login. If you turn in files generated under another login or with an empty login, your submission will be invalid.

### 3. Peer-Evaluation / Defense Format
- Only files committed to your Git repository are graded.
- During evaluation, your peer evaluator will:
  1. Verify the 10 configuration files in your repository.
  2. Ask theoretical networking questions (IP classes, masks, routers vs switches, routing tables).
  3. **Live Test**: Require you to solve **random levels** (typically 3 random levels chosen from levels 6 through 10) live on the training platform within a ~15-minute time limit to prove you understand the logic rather than memorizing numbers.

---

## 🚀 Project Overview & Quick Start

NetPractice is a visual, browser-based training simulator designed to teach you the fundamentals of **IPv4 networking**, **CIDR subnetting**, and **routing tables**.

### How to Run Locally
Ensure you have `python3` installed on your machine, then run:

```bash
cd net_practice
chmod +x run.sh
./run.sh
```
The script binds a free port and automatically launches your browser at `http://localhost:<port>`.

---

## 🧠 Cisco Networking Basics & Core Concepts

### 1. IPv4 Address Anatomy
An IPv4 address consists of **32 bits** divided into **4 octets** (8 bits each), separated by dots:
```
Example: 192 . 168 .  1  . 10
Binary:  11000000.10101000.00000001.00001010
```
Every IP address has two parts:
1. **Network Portion**: Identifies which network/island the device belongs to.
2. **Host Portion**: Identifies the specific device on that network.

### 2. Subnet Mask & CIDR Prefix
The **Subnet Mask** tells devices where the network portion ends and the host portion begins.
- A **`1`** in binary = Network bit (frozen/locked).
- A **`0`** in binary = Host bit (customizable device ID).

CIDR (*Classless Inter-Domain Routing*) notation denotes how many leading bits are `1`s:
- `/24` = `255.255.255.0` (24 ones, 8 zeros)
- `/25` = `255.255.255.128` (25 ones, 7 zeros)
- `/30` = `255.255.255.252` (30 ones, 2 zeros — ideal for point-to-point router links)

### 3. The 3 Special Addresses in Every Subnet
For any given subnet with $N$ total addresses:
- **Network ID (First Address)**: Identifies the entire subnet. **Cannot be assigned to a host.**
- **Broadcast Address (Last Address)**: Sends packets to all hosts on the subnet. **Cannot be assigned to a host.**
- **Usable Host Addresses**: Everything in between ($2^{\text{host bits}} - 2$).

### 4. Private vs. Public IP Ranges (RFC 1918)
Private IPs cannot be routed across the public Internet:
- **Class A**: `10.0.0.0` to `10.255.255.255` (`10.0.0.0/8`)
- **Class B**: `172.16.0.0` to `172.31.255.255` (`172.16.0.0/12`)
- **Class C**: `192.168.0.0` to `192.168.255.255` (`192.168.0.0/16`)

### 5. Network Devices: Switch vs. Router
| Device | OSI Layer | Role in NetPractice |
| :--- | :--- | :--- |
| **Switch** | Layer 2 (Data Link) | Connects devices **within the same subnet**. All devices connected to the same switch share the exact same network IP and mask. |
| **Router** | Layer 3 (Network) | Connects **different subnets**. Each interface on a router must belong to a **different subnet**. |

### 6. Routing Tables & Default Gateway
A routing table instructs a host or router where to send a packet:
- **Destination**: The target network address and mask (e.g., `192.168.2.0/24`).
- **Next Hop / Gateway**: The IP address of the adjacent router interface on the **local** subnet that will forward the packet.
- **Default Gateway (`0.0.0.0/0`)**: The catch-all route. If a destination is not explicitly listed, the packet is forwarded to this gateway (typically the exit router or ISP).

---

## ⚡ Subnetting Secrets & Mental Math Shortcuts

Forget manual binary conversion during tests. Use the **Magic Number Method** to solve subnets in 5 seconds.

### The Magic Number Rule
1. Identify the **interesting octet** (the octet in the subnet mask that is NOT `0` and NOT `255`).
2. Calculate:
   $$\text{Magic Number} = 256 - \text{Interesting Octet Value}$$
3. The **Magic Number** is your **block size / step size**. Networks start at multiples of this number: $0, M, 2M, 3M, \dots$

#### Example: Where does `192.168.1.135/26` belong?
1. `/26` has 2 bits in the 4th octet: $128 + 64 = 192 \implies \text{Mask} = 255.255.255.192$.
2. Interesting octet = `192`.
3. $\text{Magic Number} = 256 - 192 = \mathbf{64}$.
4. Multiples of 64 in the 4th octet: `0`, `64`, `128`, `192`.
5. `135` falls between `128` and `192`:
   - **Network Address**: `192.168.1.128`
   - **First Usable Host**: `192.168.1.129`
   - **Last Usable Host**: `192.168.1.190`
   - **Broadcast Address**: `192.168.1.191` (one before 192)

---

### 📊 The 4th Octet Cheat Sheet (/24 to /30)

| CIDR | Subnet Mask | Magic Number (Block Size) | Total IPs | Usable Hosts ($2^h - 2$) | Typical Use Case |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **/24** | `255.255.255.0` | **256** | 256 | 254 | Standard LAN |
| **/25** | `255.255.255.128` | **128** | 128 | 126 | Split a /24 in two |
| **/26** | `255.255.255.192` | **64** | 64 | 62 | Medium departmental subnet |
| **/27** | `255.255.255.224` | **32** | 32 | 30 | Small LAN |
| **/28** | `255.255.255.240` | **16** | 16 | 14 | Small server group |
| **/29** | `255.255.255.248` | **8** | 8 | 6 | Small cluster |
| **/30** | `255.255.255.252` | **4** | 4 | **2** | **Point-to-Point Router Links** |

> [!TIP]
> **The /30 Shortcut (Goldmine for NetPractice Levels 6–10)**:
> In `/30`, blocks come in multiples of 4 (`0, 4, 8, 12, 16, 20...`).
> - Block `0-3`: Net `.0`, Hosts `.1, .2`, Bcast `.3`
> - Block `4-7`: Net `.4`, Hosts `.5, .6`, Bcast `.7`
> - Block `8-11`: Net `.8`, Hosts `.9, .10`, Bcast `.11`
> Whenever you see a direct cable between two routers, immediately look for a `/30`!

---

## 🎯 NetPractice Golden Rules (The 5 Commandments)

1. **The Switch Rule**: All interfaces plugged into the same switch must have **matching network addresses and matching subnet masks**.
2. **The Router Interface Rule**: Every interface on a router connects to a distinct physical link, so each interface must belong to a **completely different subnet**. No router interface can share a subnet with another interface on that same router.
3. **No Overlapping Subnets**: A subnet cannot overlap with any other subnet in the topology. If Router 1 has `10.0.1.0/24`, no other link in the network can use addresses in `10.0.1.0 - 10.0.1.255`.
4. **The Next-Hop Neighbor Rule**: The `Next Hop` in a routing table must **always** be an IP address directly reachable on the local subnet (i.e. the immediate neighbor's interface), never an IP two hops away.
5. **Two-Way Traffic (Return Path)**: Ping (`ICMP`) requires an echo request AND an echo reply. If Host A sends a packet to Host B, verify that Host B (and all intermediate routers) has a route to get **back** to Host A!

---

## 📺 Curated YouTube Study Resources

### 1. NetworkChuck (The Best Visual Intro)
- 🎥 [NetworkChuck: Subnetting is Simple (Full Series)](https://www.youtube.com/playlist?list=PLIhvC56v63IJVXv0GJcl9vO5Z6znCVb1P)
  *Watch this first.* He explains binary math, CIDR, and why subnetting exists using coffee cups and real-world analogies.
- 🎥 [NetworkChuck: Free CCNA Course](https://www.youtube.com/playlist?list=PLIhvC56v63IKC4ahPDTLHgGKWL45U656q)

### 2. Jeremy's IT Lab (The Gold Standard for Routing & Switching)
- 🎥 [Jeremy's IT Lab: Free Complete CCNA 200-301](https://www.youtube.com/playlist?list=PLxbwE86EzrgW38n4tS544WB9InnVMU2NA)
  - **Day 4 & 5**: IPv4 Addressing & Subnetting fundamentals.
  - **Day 8 & 9**: Routing Tables, Static Routes & Default Routes (`0.0.0.0/0`).
  - **Day 11**: Switches & VLAN basics.

### 3. PowerCert Animated Videos (Quick 5-Minute Visual Explainers)
- 🎥 [Subnet Mask Explained](https://www.youtube.com/watch?v=s_Ntt6eTn94)
- 🎥 [Routers vs. Switches vs. Hubs](https://www.youtube.com/watch?v=1z0ULvg_pW8)
- 🎥 [Default Gateway Explained](https://www.youtube.com/watch?v=k-eF_9p7Q8g)

### 4. David Bombal
- 🎥 [David Bombal: Subnetting in your head](https://www.youtube.com/watch?v=vcArfZ_75qM)
  Fast mental math speed training for exams.

### 5. Professor Messer (CompTIA Network+)
- 🎥 [Seven Second Subnetting](https://www.youtube.com/watch?v=zxqyfffnx_Y)
  How to write down a simple grid at the start of any networking exam to answer any subnet question instantly.

---

## 🤖 How to Practice with AI (Interactive Prompts)

Copy and paste these prompts into your AI assistant to generate infinite practice drills and verify your solutions.

### Prompt 1: The 42 NetPractice Peer Evaluator (Oral Defense Sim)
```text
Act as a strict 42 School peer evaluator assessing me on NetPractice.
Ask me 5 technical questions one by one. Do not give the answers away.
Questions should cover:
1. Subnet mask derivation and CIDR notation.
2. Why we subtract 2 to find usable host addresses.
3. How a router decides which route to pick when multiple match (longest prefix match).
4. What 0.0.0.0/0 means and when to use it as next hop.
5. Given a specific IP and mask (e.g., 172.16.45.198/27), ask me for the network address, broadcast address, and usable host range.
Evaluate my answer after each question and provide constructive feedback.
```

### Prompt 2: Rapid-Fire Subnetting Mental Math Trainer
```text
I am practicing subnetting mental math. Give me 5 random IP/CIDR pairs (mix of /25, /26, /27, /28, /29, and /30).
For each pair, wait for my response where I provide:
- Netmask in dotted decimal
- Network address
- Usable host range (First - Last)
- Broadcast address
Grade my calculations and explain any mistakes using the Magic Number method.
```

### Prompt 3: Topology Debugger
```text
I am solving a NetPractice level and my packets are not reaching the destination.
Here is my current topology configuration:
- Host A: IP [x.x.x.x], Mask [x.x.x.x], Default Gateway [x.x.x.x]
- Router R1 Interface 1: IP [x.x.x.x], Mask [x.x.x.x]
- Router R1 Interface 2: IP [x.x.x.x], Mask [x.x.x.x]
- Routing table for R1: [Destination -> Next Hop]
- Host B: IP [x.x.x.x], Mask [x.x.x.x], Default Gateway [x.x.x.x]

Analyze this step by step:
1. Check if Host A and R1-eth1 are on the same subnet.
2. Check if R1's interfaces overlap.
3. Check forward path from Host A to Host B.
4. Check return path from Host B back to Host A.
Point out exactly which field is causing the packet drop without spoiling the entire solution.
```

---

## 🎓 42 Peer Evaluation & Defense Prep

During your peer review, you must be able to justify every single number you typed in. Review these common traps:

1. **"Why can't I set an interface IP to .0 or .255?"**
   - In a `/24`, `.0` is the network address and `.255` is the broadcast address. However, remind your peer that in `/23` or `/22`, `.255` CAN be a valid host IP (e.g. `10.0.0.255/23` is a usable host, because the broadcast is `10.0.1.255`)!
2. **"What is the longest prefix match?"**
   - If a router has routes for both `192.168.1.0/24` and `192.168.0.0/16`, a packet for `192.168.1.50` will match the `/24` route because `/24` is more specific (longer prefix).
3. **"Why did level 10 fail when all IP addresses are green?"**
   - Check the **return route**. Sending the packet is only half the battle; the destination host must know how to route responses back to the sender.
4. **"Why is the next-hop field empty in some routes?"**
   - When a network is directly connected to the router's own interface, no next-hop IP is needed (or it routes directly via interface). Next hop is only needed to reach remote networks through neighboring gateways.

