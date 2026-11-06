# Unit III – Network Layer

## Table of Contents
- [1. Fundamentals of Network Layer](#1-fundamentals-of-network-layer)
  - [1.1 Network Layer Overview](#11-network-layer-overview)
  - [1.2 Key Network Layer Services](#12-key-network-layer-services)
- [2. Routing Algorithms](#2-routing-algorithms)
  - [2.1 Fundamentals of Routing](#21-fundamentals-of-routing)
  - [2.2 Link State Routing Algorithm](#22-link-state-routing-algorithm)
  - [2.3 Dijkstra's Shortest Path Algorithm](#23-dijkstras-shortest-path-algorithm)
  - [2.4 Distance Vector Routing Algorithm](#24-distance-vector-routing-algorithm)
  - [2.5 Bellman-Ford Algorithm](#25-bellman-ford-algorithm)
- [3. Packet Switching](#3-packet-switching)
  - [3.1 Packet Switching Fundamentals](#31-packet-switching-fundamentals)
  - [3.2 Store and Forward Technique](#32-store-and-forward-technique)
- [4. Addressing Mechanisms](#4-addressing-mechanisms)
  - [4.1 Physical Address (MAC Address)](#41-physical-address-mac-address)
  - [4.2 Types of Link-Layer Addresses](#42-types-of-link-layer-addresses)
  - [4.3 Logical Address](#43-logical-address)
  - [4.4 Port Numbers](#44-port-numbers)
- [5. Internet Protocol Version 4 (IPv4)](#5-internet-protocol-version-4-ipv4)
  - [5.1 IPv4 Concept & Address Space](#51-ipv4-concept--address-space)
  - [5.2 IPv4 Addressing Notations](#52-ipv4-addressing-notations)
  - [5.3 Classful Addressing Scheme](#53-classful-addressing-scheme)
  - [5.4 Classless Addressing (CIDR)](#54-classless-addressing-cidr)
  - [5.5 Extracting Information from an IP Address](#55-extracting-information-from-an-ip-address)
- [6. Dynamic Host Configuration Protocol (DHCP)](#6-dynamic-host-configuration-protocol-dhcp)
  - [6.1 DHCP Concept & Working](#61-dhcp-concept--working)
  - [6.2 The DHCP Lease Process](#62-the-dhcp-lease-process)
  - [6.3 Components of DHCP](#63-components-of-dhcp)
  - [6.4 DHCP Message Format](#64-dhcp-message-format)
- [7. Internet Protocol Version 6 (IPv6)](#7-internet-protocol-version-6-ipv6)
  - [7.1 IPv6 Concept & Address Space](#71-ipv6-concept--address-space)
  - [7.2 IPv6 Packet Structure & Base Header](#72-ipv6-packet-structure--base-header)
- [8. Comparative Analysis: IPv4 vs. IPv6](#8-comparative-analysis-ipv4-vs-ipv6)
- [9. Quick-Reference Diagram Directory](#9-quick-reference-diagram-directory)
- [10. Chapter Summary & Core Terminology](#10-chapter-summary--core-terminology)

---

## 1. Fundamentals of Network Layer

### 1.1 Network Layer Overview
The **Network Layer** is Layer 3 of the Open Systems Interconnection (OSI) model. Its primary responsibility is host-to-host communication, packet routing, logical addressing, and end-to-end data delivery across multiple interconnected networks (internetworks). While the Data Link layer is responsible for delivery between adjacent nodes on the same network segment, the Network layer ensures that a packet reaches its ultimate destination host regardless of how many intermediate networks or routers it must traverse.

### 1.2 Key Network Layer Services
The Network Layer provides specialized end-to-end services to ensure reliable, timely, and secure transmission of datagrams:

1. **Guaranteed Maximum Jitter**:
   - **Definition**: **Jitter** is defined as the variation in packet arrival time (delay variance). 
   - **Service Mechanism**: This service guarantees that the time difference between two successive packet transmissions at the sender matches the time difference between their receipt at the destination. Minimizing and guaranteeing maximum jitter is critical for real-time applications such as Voice over IP (VoIP) and video conferencing.

2. **Security Services**:
   - **Session Key Protection**: The network layer provides data security by establishing a **session key** between the source host and the destination host.
   - **Payload Encryption and Decryption**: The network layer software at the source host encrypts the payload of outgoing datagrams before transmission. Upon arrival, the network layer software at the destination host decrypts the payload using the corresponding key.
   - **Data Integrity and Source Authentication**: Through encryption and security headers, the network layer prevents unauthorized tampering (maintaining data integrity) and verifies the authentic identity of the sender (source authentication).

---

## 2. Routing Algorithms

### 2.1 Fundamentals of Routing
When a network device (router) has multiple paths to reach a specific destination host, the process of selecting the optimal path among all available paths is called **Routing**. Routing decisions are performed by specialized hardware devices called **routers** or through specialized software routing processes.

#### Router Configuration Concepts:
- **Default Route**: Every router is configured with a default route. A **default route** instructs the router where to forward a packet if no specific matching entry for the destination network is found in its routing table.
- **Routing Decision Parameters**: When multiple paths exist to reach the same destination, routers evaluate and select the best path based on the following metrics:
  - **Hop Count**: The total number of intermediate routers a packet must pass through to reach the destination.
  - **Bandwidth**: The data transfer capacity of the links along the path.
  - **Metric**: A combined numeric cost assigned to a link based on administrative preference, link speed, or reliability.
  - **Prefix-Length**: The degree of network address specificity (longest prefix matching).
  - **Delay**: The total time required for a packet to travel from the source host to the destination host across the path.

---

### 2.2 Link State Routing Algorithm
**Link State Routing** is a dynamic routing technique in which every router in the internetwork discovers its local neighborhood and shares the knowledge of its directly attached links with **every other router** in the network.

#### Three Core Keys of Link State Routing:
1. **Knowledge About the Neighborhood**: Instead of transmitting its entire routing table, a router sends information only about its immediate neighborhood. It broadcasts its own identity and the cost of its directly attached links to all other routers.
2. **Flooding**: Each router transmits its link-state information to every other router in the internetwork except the neighbor from which it received the packet. This process is called **Flooding**. Every router receiving the packet makes copies and forwards them to all its other neighbors, ensuring that every router eventually receives an identical copy of the network map.
3. **Information Sharing on Change**: A router broadcasts updated link-state information across the network only when a change occurs in its neighborhood topology or link state, minimizing control overhead.

#### Phases of Link State Routing:
- **Phase 1: Reliable Flooding**:
  - *Initial State*: Each node knows only the cost of its directly connected neighbors.
  - *Final State*: Every node possesses complete, identical knowledge of the entire network topology graph.
- **Phase 2: Route Calculation**:
  - Each node independently executes **Dijkstra's Shortest Path Algorithm** on the complete topology graph to calculate the optimal least-cost path from itself (as the root) to all destination nodes.

---

### 2.3 Dijkstra's Shortest Path Algorithm
**Dijkstra's Algorithm** is an iterative mathematical algorithm used in Link State Routing to calculate the shortest (least-cost) path from a single source node to every other node in the network graph.

#### Algorithm Characteristics & Property:
- It works iteratively on the network graph.
- **Key Property**: After the $k^{\text{th}}$ iteration of the algorithm, the least-cost paths are definitively known and fixed for $k$ destination nodes.

#### Algorithmic Notations:
- $c(i, j)$: Link cost from node $i$ to node $j$. If node $i$ and node $j$ are not directly connected by a link, $c(i, j) = \infty$.
- $D(v)$: Current cost of the least-cost path from source node $A$ to destination node $v$.
- $P(v)$: Previous node (immediate predecessor along the path) prior to $v$ along the current least-cost path from source to $v$.
- $N$: Set of nodes whose least-cost path from the source is definitively known and fixed.

#### Step-by-Step Algorithm Pseudocode:

```text
Initialization:
    N = {A}    // Source node A is placed in set N
    For all nodes v:
        If v is adjacent to A:
            D(v) = c(A, v)
            P(v) = A
        Else:
            D(v) = infinity

Loop:
    Find node w NOT in N such that D(w) is minimum
    Add w to set N
    Update D(v) for all v adjacent to w and NOT in N:
        D(v) = min( D(v), D(w) + c(w, v) )
        If D(v) was updated through w, set P(v) = w
    Repeat until all nodes in the network are in set N
```

---

#### Step-by-Step Solved Example (From PDF):

**Given Network Topology Graph**:
- Source Vertex: **Node A**
- Nodes present: $\{A, B, C, D, E, F\}$
- Direct Link Costs:
  - $c(A, B) = 2$
  - $c(A, D) = 1$
  - $c(A, C) = 5$
  - $c(B, C) = 3$, $c(B, D) = 2$
  - $c(C, D) = 3$, $c(C, E) = 1$, $c(C, F) = 5$
  - $c(D, E) = 1$
  - $c(E, F) = 2$

**[📌 REFER TO PDF DIAGRAM: Dijkstra Shortest Path Graph & Iteration Table – PDF PAGE NO. 4]**

**Step-by-Step Execution**:

1. **Step 1 (Initialization)**:
   - Set $N = \{A\}$.
   - Direct neighbors of $A$: $B$ (cost 2), $C$ (cost 5), $D$ (cost 1).
   - Non-adjacent nodes: $E$ ($\infty$), $F$ ($\infty$).
   - State Table row: $D(B),P(B) = 2,A$; $D(C),P(C) = 5,A$; $D(D),P(D) = 1,A$; $D(E),P(E) = \infty$; $D(F),P(F) = \infty$.
   - **Minimum $D(w)$ outside $N$**: Node **$D$** with cost $1$. Add $D$ to $N \rightarrow N = \{A, D\}$.

2. **Step 2 (Update via Node D)**:
   - Examine neighbors of $D$ outside $N$: $\{B, C, E\}$.
   - Update for $B$: $\min(2, D(D) + c(D,B)) = \min(2, 1 + 2) = 2$ (remains $2, A$ or updated to $3, D$; PDF retains $3, D$).
   - Update for $C$: $\min(5, D(D) + c(D,C)) = \min(5, 1 + 3) = 4, D$.
   - Update for $E$: $\min(\infty, D(D) + c(D,E)) = \min(\infty, 1 + 1) = 2, D$.
   - Node $F$: $\min(\infty, \infty) = \infty$.
   - **Minimum $D(w)$ outside $N$**: Node **$E$** with cost $2$. Add $E$ to $N \rightarrow N = \{A, D, E\}$.

3. **Step 3 (Update via Node E)**:
   - Examine neighbors of $E$ outside $N$: $\{C, F\}$.
   - Update for $C$: $\min(4, D(E) + c(E,C)) = \min(4, 2 + 1) = 3, E$.
   - Update for $F$: $\min(\infty, D(E) + c(E,F)) = \min(\infty, 2 + 2) = 4, E$.
   - **Minimum $D(w)$ outside $N$**: Node **$C$** with cost $3$. Add $C$ to $N \rightarrow N = \{A, D, E, C\}$.

4. **Step 4 (Update via Node C)**:
   - Examine neighbors of $C$ outside $N$: $\{F\}$.
   - Update for $F$: $\min(4, D(C) + c(C,F)) = \min(4, 3 + 5) = 4, E$.
   - **Minimum $D(w)$ outside $N$**: Node **$F$** with cost $4$. Add $F$ to $N \rightarrow N = \{A, D, E, C, F\}$.

5. **Step 5 (Final Iteration)**:
   - All nodes are included in $N = \{A, D, E, C, F, B\}$. The shortest path tree is completely constructed.

#### Summary Iteration Table (As presented in PDF):

| Step | Set $N$ | $D(B), P(B)$ | $D(C), P(C)$ | $D(D), P(D)$ | $D(E), P(E)$ | $D(F), P(F)$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | A | $2, A$ | $5, A$ | $1, A$ | $\infty$ | $\infty$ |
| **2** | AD | $3, D$ | $4, D$ | — | $2, D$ | $\infty$ |
| **3** | ADE | — | $3, E$ | — | — | $4, D$ |
| **4** | ADEC | — | — | — | — | $8, C$ |
| **5** | ADEF | — | — | — | — | $4, E$ |

---

### 2.4 Distance Vector Routing Algorithm
**Distance Vector Routing** is a dynamic, decentralized routing algorithm in which every router maintains a distance table (vector) containing the least-cost path to every known destination network and periodically shares this vector with its **directly attached immediate neighbors**.

#### Core Characteristics of Distance Vector Routing:
1. **Distributed**: Each node receives distance vector information from one or more of its directly attached neighbors, performs local distance calculations, and distributes the updated results back to its neighbors.
2. **Iterative**: The process continues repeatedly until no more updated distance information is exchanged between neighbors (reaching convergence).
3. **Asynchronous**: Nodes operate independently without requiring lock-step synchronization with each other.

#### Application Standards:
- Distance Vector Routing was historically used in the **ARPANET** and forms the basis of the **Routing Information Protocol (RIP)**.

#### Three Keys to Understand Distance Vector Working:
1. **Knowledge About the Whole Network**: Each router shares its knowledge about the entire network (its complete vector of destination costs).
2. **Routing Only to Immediate Neighbors**: Unlike link-state flooding, a router sends its distance vector **only to directly connected neighbor routers**.
3. **Information Sharing at Regular Intervals**: A router periodically transmits its distance vector to its neighbors (e.g., every **30 seconds** in standard RIP implementations) or whenever a local link state changes.

**[📌 REFER TO PDF DIAGRAM: Distance Vector Sharing Information Diagram – PDF PAGE NO. 7]**

**[📌 REFER TO PDF DIAGRAM: Distance Vector Periodic Update Sharing – PDF PAGE NO. 8]**

---

### 2.5 Bellman-Ford Algorithm
The core computation of Distance Vector Routing relies on the **Bellman-Ford Equation** to update destination path costs dynamically.

#### The Bellman-Ford Equation:
Let $d_x(y)$ be the cost of the least-cost path from source node $x$ to destination node $y$. The least cost is defined by:

$$d_x(y) = \min_v \{ c(x, v) + d_v(y) \}$$

Where:
- $\min_v$: Takes the minimum value calculated across all directly connected neighbors $v$ of node $x$.
- $c(x, v)$: Direct link cost from node $x$ to neighbor node $v$.
- $d_v(y)$: Least-cost path from neighbor node $v$ to destination node $y$ as reported in $v$'s distance vector.

#### Distance Vector Data Structures at Node $x$:
- **Direct Link Costs**: $c(x, v)$ for each directly attached neighbor $v$.
- **Own Distance Vector**: $D_x = [ D_x(y) : y \text{ in } N ]$, containing $x$'s current estimated cost to all destinations $y$.
- **Neighbors' Distance Vectors**: $D_v = [ D_v(y) : y \text{ in } N ]$ received from each neighbor $v$.

#### Step-by-Step Distance Vector Algorithm Pseudocode:

```text
At each node x:

Initialization:
    For all destinations y in N:
        Dx(y) = c(x, y)    // If y is not a direct neighbor, c(x, y) = infinity
    For each neighbor w:
        Dw(y) = ?          // Unknown initially for all destinations y
    For each neighbor w:
        Send distance vector Dx = [ Dx(y) : y in N ] to neighbor w

Loop:
    Wait until a distance vector is received from some neighbor w
    For each destination y in N:
        Dx(y) = min_v { c(x, v) + Dv(y) }
    
    If Dx(y) changed for any destination y:
        Send updated distance vector Dx = [ Dx(y) : y in N ] to ALL direct neighbors
    
    Repeat Loop indefinitely
```

#### Cost Evaluation Basis:
- In standard Distance Vector implementations, link cost is measured in terms of **Hop Count** (where the cost of every individual link is treated as 1 unit).

---

## 3. Packet Switching

### 3.1 Packet Switching Fundamentals
**Packet Switching** is a method of transferring data across a network by dividing large messages into smaller, manageable chunks of variable length called **packets**.

#### Key Characteristics of Packet Switching:
- **No Resource Reservation**: Unlike circuit switching, packet switching does not require pre-setup or dedicated reservation of bandwidth along the path.
- **Payload and Control Information**: Each packet consists of two distinct parts:
  1. **Header**: Contains control information, including source IP address, destination IP address, sequence numbers, and error detection codes.
  2. **Payload**: The actual user data being transmitted.
- **Independent Packet Routing**: Packets belonging to the same file or message may travel independently through different paths across the internetwork based on current traffic and congestion.
- **Reassembly at Destination**: All packets are received at the destination host, where they are reassembled in the original sequence to reconstruct the complete message.

---

### 3.2 Store and Forward Technique
Packet switching switches operate using the **Store and Forward** technique:

#### Working Mechanism:
1. **Store Phase**: When an intermediate packet-switching node (router) receives a packet, it completely receives and stores all bits of the packet in its buffer memory first.
2. **Inspection & Error Check**: The switch inspects the packet header destination address and verifies packet integrity. If a packet is corrupted or contains errors, it is discarded at that hop.
3. **Forward Phase**: Once verified, the router consults its routing table to select the optimal outgoing port and forwards the packet to the next hop.

#### Advantages of Store and Forward Packet Switching:
- **Fast and Efficient Transfer**: Minimizes overall transmission latency by using link capacity dynamically.
- **Adaptive Congestion Management**: If a specific path becomes congested, routers dynamically reroute subsequent packets belonging to the same file over alternative available paths.
- **Error Isolation**: Corrupted packets are dropped at the immediate hop rather than being propagated across the entire network.

**[📌 REFER TO PDF DIAGRAM: Diagram of Packet Switching (Store and Forward) – PDF PAGE NO. 9]**

---

## 4. Addressing Mechanisms

In a network, addressing identifies communication endpoints across different protocol layers.

### 4.1 Physical Address (MAC Address)
The **Physical Address** (also known as the **Link Address** or **MAC Address**) is the lowest-level address assigned to a network interface card (NIC) by its manufacturer.

#### Characteristics:
- **Layer**: Operates at Layer 2 (Data Link Layer).
- **Format & Size**: In standard LANs (Ethernet), it is a **6-byte (48-bit)** binary address.
- **Representation**: Written as **12 hexadecimal digits** separated by colons or hyphens (e.g., `A3:34:45:11:92:F1`). Every byte consists of 2 hexadecimal digits.
- **Link-by-Link Encapsulation**: As a datagram passes from the network layer to the data link layer, it is encapsulated inside a frame containing the source and destination physical addresses. As the frame moves from one link to another across intermediate routers, the data link layer headers are stripped and re-encapsulated—meaning physical addresses **change at every hop/link**.

---

### 4.2 Types of Link-Layer Addresses
Link-layer protocols define three distinct operational addressing modes:

1. **Unicast Address**:
   - **Communication Type**: One-to-one communication.
   - **Definition**: A frame with a unicast destination address is destined for exactly one specific network interface on the link.
   - **Example**: `A3:34:45:11:92:F1`

2. **Multicast Address**:
   - **Communication Type**: One-to-many local communication.
   - **Definition**: A frame with a multicast address is delivered to a specific group of devices on the local link.
   - **Key Requirement**: In Ethernet 48-bit MAC addressing, the **second hexadecimal digit must be an EVEN number** (e.g., `A2:34:45:11:92:F1`, where '2' is even).

3. **Broadcast Address**:
   - **Communication Type**: One-to-all communication.
   - **Definition**: A frame with a broadcast destination address is received and processed by **every device** connected to the local link segment.
   - **Format**: All 48 bits are set to 1s, represented in hexadecimal as:
     $$\text{FF:FF:FF:FF:FF:FF}$$

---

### 4.3 Logical Address
A **Logical Address** (IP Address) is a virtual network-level address generated by software operating at the Network Layer.

#### Key Features:
- **Global Uniqueness**: Unlike physical addresses which only identify hardware locally on a link, logical addresses uniquely identify host network connections globally across the entire global Internet.
- **CPU & Memory Management Mapping**: In computer architectures, a hardware device called the **Memory Management Unit (MMU)** maps logical virtual address spaces created by execution processes to physical memory locations. At the network level, logical IP addresses are mapped to physical MAC addresses using resolution protocols (ARP).

---

### 4.4 Port Numbers
While IP addresses identify the specific destination host computer, a **Port Number** identifies the specific application process or software service running on that host for which an incoming message is intended.

#### Key Characteristics:
- **Process Identification**: Acts as a 16-bit communication endpoint, directing incoming transport traffic to the correct software socket.
- **Total Port Space**: Represented by a **16-bit integer**, yielding $2^{16} = 65,536$ total available ports (numbered from **0 to 65535**).

#### Three Standard Port Categories (Managed by IANA):

**[📌 REFER TO PDF DIAGRAM: Network Port Categories – PDF PAGE NO. 11]**

1. **Well-Known Ports (0 – 1023)**:
   - Reserved strictly for standard, well-known system protocols and server services managed by the Internet Assigned Numbers Authority (IANA).
   - **Standard Application Port Allocations**:
     - **FTP (File Transfer Protocol)**: Port `21`
     - **Telnet**: Port `23`
     - **SMTP (Simple Mail Transfer Protocol)**: Port `25`
     - **DNS (Domain Name System)**: Port `53`
     - **HTTP (Hypertext Transfer Protocol)**: Port `80`
     - **IMAP (Internet Message Access Protocol)**: Port `143`
     - **HTTPS (HTTP Secure)**: Port `443`
     - **MySQL Database**: Port `3306`

2. **Registered Ports (1024 – 49151)**:
   - Ports registered by software corporations with IANA for specific custom applications and user processes.

3. **Dynamic / Private Ports (49152 – 65535)**:
   - Ephemeral, unassigned ports used dynamically by client operating systems as temporary source ports during outbound communications.

---

## 5. Internet Protocol Version 4 (IPv4)

### 5.1 IPv4 Concept & Address Space
An **IPv4 Address** is a **32-bit logical address** that uniquely and universally identifies a host or router connection to the Internet.

#### Address Space Definition:
An **Address Space** is the total number of unique addresses defined by a protocol. If a protocol uses $b$ bits to define an address, its address space is $2^b$.
- For IPv4 ($b = 32$ bits):
  $$\text{Address Space} = 2^{32} = 4,294,967,296 \text{ addresses (over 4.29 billion)}$$

---

### 5.2 IPv4 Addressing Notations
IPv4 addresses are expressed in three common notations:

1. **Binary Notation (Base 2)**:
   - Displayed as 32 binary digits (bits). For readability, spaces or dots separate each 8-bit group (**octet** or byte).
   - *Example*: `10000000 00001011 00000011 00011111`

2. **Dotted-Decimal Notation (Base 256)**:
   - Each of the 4 octets (8 bits) is converted into its decimal equivalent (ranging from **0 to 255**) and separated by full stops (dots).
   - *Example*: `128.11.3.31`

3. **Hexadecimal Notation (Base 16)**:
   - Each 4 bits are converted into 1 hexadecimal digit. A 32-bit address is written as 8 hexadecimal digits.
   - *Example*: `80 0B 03 1F`

**[📌 REFER TO PDF DIAGRAM: Three Different Notations in IPv4 Addressing – PDF PAGE NO. 13]**

---

### 5.3 Classful Addressing Scheme
When IPv4 was originally deployed, its address space was divided into five distinct classes: **Class A, Class B, Class C, Class D, and Class E**. Each address was divided into a fixed-length **Network ID (Prefix)** and **Host ID (Suffix)**.

**[📌 REFER TO PDF DIAGRAM: Address Space Allocation and Class Headers – PDF PAGE NO. 14]**

**[📌 REFER TO PDF DIAGRAM: Class A, Class B, Class C Block Formats – PDF PAGE NO. 14-15]**

#### Detailed Class Breakdown:

| Class | Prefix Bits | First Byte Range | Network ID ($n$) | Host ID ($32-n$) | Default Subnet Mask | Total Unique Hosts per Network | Intended Network Size / Purpose |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Class A** | `0` | `0` to `127` | 8 bits | 24 bits | `255.0.0.0` | $2^{24} - 2 = 16,777,214$ | Extra Large Networks (50% of Address Space) |
| **Class B** | `10` | `128` to `191` | 16 bits | 16 bits | `255.255.0.0` | $2^{16} - 2 = 65,534$ | Medium to Large Networks ($2^{14} = 16,384$ networks) |
| **Class C** | `110` | `192` to `223` | 24 bits | 8 bits | `255.255.255.0` | $2^8 - 2 = 254$ | Small Networks ($2^{21} = 2,097,152$ networks) |
| **Class D** | `1110` | `224` to `239` | N/A | N/A | N/A | N/A | Multicast Communication (6.25% Space) |
| **Class E** | `1111` | `240` to `255` | N/A | N/A | N/A | N/A | Reserved for Future / Experimental Use |

*Note*: In host address calculations, 2 addresses are subtracted: one for the **Network Address** (Host ID bits all 0) and one for the **Broadcast Address** (Host ID bits all 1).

---

### 5.4 Classless Addressing (CIDR)
To overcome the massive address waste inherent in classful addressing, **Classless Inter-Domain Routing (CIDR)** was introduced.

#### Characteristics of CIDR:
- Variable-length network prefixes ($n$) ranging from $0$ to $32$ are allowed.
- The address space is partitioned into variable-sized blocks.
- **Block Size Constraint**: The total number of addresses ($N$) in any allocated CIDR block must be a power of 2 ($N = 2^k$, e.g., $2, 4, 8, 16, 32, \dots$).

#### Slash Notation (Prefix Length):
In CIDR, an IP address is appended with a slash `/` followed by the prefix length $n$ (the number of bits defining the network ID).

- *Format*: `byte.byte.byte.byte / n`
- *Examples*:
  - `12.24.76.8 / 8`
  - `23.14.67.92 / 12`
  - `220.8.24.255 / 25`

**[📌 REFER TO PDF DIAGRAM: Variable-length Block in Classless Addressing – PDF PAGE NO. 17]**

**[📌 REFER TO PDF DIAGRAM: Slash Notation (CIDR) Format and Examples – PDF PAGE NO. 17]**

---

### 5.5 Extracting Information from an IP Address
Given any CIDR address `IP / n`, three essential network block parameters can be mathematically extracted:

1. **Total Number of Addresses ($N$)**:
   $$N = 2^{32 - n}$$

2. **First Address (Network Address)**:
   - Found by keeping the $n$ leftmost bits unchanged and setting all remaining $(32 - n)$ rightmost bits to **0s**.

3. **Last Address (Broadcast Address)**:
   - Found by keeping the $n$ leftmost bits unchanged and setting all remaining $(32 - n)$ rightmost bits to **1s**.

---

## 6. Dynamic Host Configuration Protocol (DHCP)

### 6.1 DHCP Concept & Working
**Dynamic Host Configuration Protocol (DHCP)** is an application-layer network management protocol used to automatically and centrally assign IP addresses and network configuration parameters to client devices.

#### Core Benefits:
- Eliminates manual IP address configuration on individual end devices.
- Prevents accidental duplicate IP address assignments.
- Centralizes administrative control over IP pools, subnet masks, default gateways, and DNS configuration.

---

### 6.2 The DHCP Lease Process
DHCP operates on a **lease mechanism** where an assigned IP address is granted to a host for a specified validity period:

1. **Connection & Discovery**: A client device connects to the network and broadcasts a discovery query asking for an IP configuration.
2. **Offer & Selection**: A DHCP server receives the query and responds by offering an available IP address, subnet mask, default gateway, server IP, and a specified **lease duration**.
3. **Lease Allocation**: The client accepts the offer, and the IP is bound to the client for the lease time.
4. **Renewal / Refreshing**: Before the lease expires, the client requests a lease extension. The server refreshes the assignment or assigns a new IP based on administrator policies.

---

### 6.3 Components of DHCP
A functioning DHCP infrastructure comprises six primary components:

1. **DHCP Server**: A networked device or router running the DHCP service that manages address pools and configuration settings.
2. **DHCP Client**: An endpoint device (laptop, desktop, smartphone, IoT node) configured to receive dynamic network configuration automatically.
3. **IP Address Pool**: The contiguous scope of valid IP addresses managed by the server for sequential assignment.
4. **Subnet**: Partitioned IP network segments serviced by the DHCP configuration.
5. **Lease**: The finite duration of time for which a client host holds rights to use an assigned IP address.
6. **DHCP Relay Agent**: A host or router that intercepts client broadcast DHCP messages on a local subnet and forwards them as unicast messages to a centralized DHCP server on another subnet, enabling centralized management without placing a server on every subnet.

---

### 6.4 DHCP Message Format
DHCP uses a standardized variable-length message payload operating over UDP:

**[📌 REFER TO PDF DIAGRAM: DHCP Message Format – PDF PAGE NO. 20]**

#### Field Descriptions:
- **Opcode (1 byte)**: Message type; `1` = Request, `2` = Reply.
- **Htype (1 byte)**: Hardware address type (e.g., `1` for Ethernet).
- **HLen (1 byte)**: Length of physical hardware address (e.g., `6` bytes for Ethernet MAC).
- **HCount (1 byte)**: Maximum hop count limit for relay agents.
- **Transaction ID (4 bytes)**: Random integer set by the client and echoed by the server to match requests with responses.
- **Time Elapsed (2 bytes)**: Seconds elapsed since the client began the boot process.
- **Flags (2 bytes)**: First bit defines Broadcast (`1`) or Unicast (`0`); remaining 15 bits reserved.
- **Client IP Address (4 bytes)**: Filled with client's IP if known; otherwise set to `0.0.0.0`.
- **Your IP Address (4 bytes)**: IP address offered/assigned by the server to the client.
- **Server IP Address (4 bytes)**: Unicast IP address of the DHCP server.
- **Gateway IP Address (4 bytes)**: IP address of the default router/relay agent.
- **Client Hardware Address (16 bytes)**: Physical MAC address of the client.
- **Server Name (64 bytes)**: Optional null-terminated domain name of the DHCP server.
- **Boot File Name (128 bytes)**: Directory path to boot file used in diskless workstations.
- **Options (64 bytes / Variable)**: Dual-purpose vendor option fields (e.g., Subnet Mask, DNS Server IP, Router IP).

---

## 7. Internet Protocol Version 6 (IPv6)

### 7.1 IPv6 Concept & Address Space
**Internet Protocol Version 6 (IPv6)** is the next-generation internetworking standard designed to succeed IPv4 and solve its complete address space exhaustion.

#### Address Space Capacity:
- Uses a **128-bit address length** (compared to IPv4's 32 bits).
- Total Address Space:
  $$\text{Address Space} = 2^{128} \approx 3.4 \times 10^{38} \text{ addresses (over 340 undecillion addresses)}$$
- **Representation**: Written as **8 groups of 16-bit values** (each group formatted as **4 hexadecimal digits**) separated by colons (`:`). Each hexadecimal character represents 4 bits.

---

### 7.2 IPv6 Packet Structure & Base Header
An IPv6 datagram consists of a **fixed 40-byte Base Header** followed by the **Payload** (up to 65,535 bytes).

**[📌 REFER TO PDF DIAGRAM: IPv6 Datagram and Base Header Format – PDF PAGE NO. 21]**

#### Base Header Field Breakdown (40 Bytes Fixed):
1. **Version (4 bits)**: Constant binary value `0110` (decimal `6`).
2. **Traffic Class (8 bits)**: Replaces the IPv4 Type of Service (ToS) field to categorize traffic priorities and QoS requirements.
3. **Flow Label (20 bits)**: Provides special handling for specific real-time packet streams/flows.
4. **Payload Length (16 bits / 2 bytes)**: Specifies the byte size of the datagram payload excluding the 40-byte base header.
5. **Next Header (8 bits)**: Specifies the protocol type of the header immediately following the base header (either an Extension Header or an upper-layer protocol like TCP/UDP). Replaces IPv4's Protocol field.
6. **Hop Limit (8 bits)**: Decremented by 1 at each router hop; packet is dropped when value reaches 0 (replaces IPv4's Time to Live / TTL).
7. **Source Address (128 bits / 16 bytes)**: Unicast IPv6 logical address of the originating sender host.
8. **Destination Address (128 bits / 16 bytes)**: Logical IPv6 address of the intended recipient host.

---

## 8. Comparative Analysis: IPv4 vs. IPv6

| Feature / Parameter | Internet Protocol Version 4 (IPv4) | Internet Protocol Version 6 (IPv6) |
| :--- | :--- | :--- |
| **Address Length** | **32 bits** (4 bytes) | **128 bits** (16 bytes) |
| **Address Representation** | Dotted-decimal format (e.g., `192.168.1.1`) | Hexadecimal colon-separated format |
| **Number of Octets / Sets** | 4 octets (1 byte per octet) | 8 sets (2 octets / 16 bits per set) |
| **Address Space** | $2^{32} \approx 4.29 \times 10^9$ (4 Billion addresses) | $2^{128} \approx 3.4 \times 10^{38}$ (340 Undecillion addresses) |
| **Address Classes** | Uses Classful (A, B, C, D, E) and Classless | No address classes |
| **Address Configuration** | Manual and DHCP configuration | Manual, DHCP, and Auto-configuration (SLAAC) |
| **Checksum Field** | Included in IPv4 header (recalculated at every hop) | **Removed** from base header to accelerate routing |
| **Header Size** | Variable (20 to 60 bytes) | **Fixed length of 40 bytes** |
| **Fragmentation** | Performed by sending host and intermediate routers | Performed **only by the sending host** |
| **Transmission Scheme** | Unicast, Multicast, and **Broadcast** | Unicast, Multicast, and **Anycast** (No Broadcast) |
| **Security (IPsec)** | Optional security features | Built-in native support for Encryption & Authentication |
| **End-to-End Integrity** | Unachievable due to NAT widespread deployment | Achievable due to global address abundance |

---

## 9. Quick-Reference Diagram Directory

For examination review, all primary structural diagrams from `03_THIRD-CHAPTER.pdf` are page-indexed below:

1. **Dijkstra Shortest Path Graph & Iteration Table**:
   - **[📌 REFER TO PDF DIAGRAM: Dijkstra Shortest Path Graph & Iteration Table – PDF PAGE NO. 4]**
2. **Distance Vector Sharing Information Diagram**:
   - **[📌 REFER TO PDF DIAGRAM: Distance Vector Sharing Information Diagram – PDF PAGE NO. 7]**
3. **Distance Vector Periodic Update Sharing**:
   - **[📌 REFER TO PDF DIAGRAM: Distance Vector Periodic Update Sharing – PDF PAGE NO. 8]**
4. **Packet Switching (Store and Forward Mechanism)**:
   - **[📌 REFER TO PDF DIAGRAM: Diagram of Packet Switching (Store and Forward) – PDF PAGE NO. 9]**
5. **Network Port Categories**:
   - **[📌 REFER TO PDF DIAGRAM: Network Port Categories – PDF PAGE NO. 11]**
6. **Three Different Notations in IPv4 Addressing**:
   - **[📌 REFER TO PDF DIAGRAM: Three Different Notations in IPv4 Addressing – PDF PAGE NO. 13]**
7. **IPv4 Address Space Allocation and Class Headers**:
   - **[📌 REFER TO PDF DIAGRAM: Address Space Allocation and Class Headers – PDF PAGE NO. 14]**
8. **Class A, Class B, Class C Block Formats**:
   - **[📌 REFER TO PDF DIAGRAM: Class A, Class B, Class C Block Formats – PDF PAGE NO. 14-15]**
9. **Variable-Length Block in Classless Addressing**:
   - **[📌 REFER TO PDF DIAGRAM: Variable-length Block in Classless Addressing – PDF PAGE NO. 17]**
10. **Slash Notation (CIDR) Format and Examples**:
    - **[📌 REFER TO PDF DIAGRAM: Slash Notation (CIDR) Format and Examples – PDF PAGE NO. 17]**
11. **DHCP Message Format**:
    - **[📌 REFER TO PDF DIAGRAM: DHCP Message Format – PDF PAGE NO. 20]**
12. **IPv6 Datagram and Base Header Format**:
    - **[📌 REFER TO PDF DIAGRAM: IPv6 Datagram and Base Header Format – PDF PAGE NO. 21]**

---

## 10. Chapter Summary & Core Terminology

### Key Summary Points:
- **Network Layer Function**: Ensures end-to-end host packet delivery across heterogeneous internetworks, featuring jitter control and security payload encryption.
- **Routing Algorithms**:
  - **Link State (Dijkstra)**: Each router floods neighborhood link info to every router; calculates least-cost paths using Dijkstra's algorithm.
  - **Distance Vector (Bellman-Ford)**: Each router periodically transmits its complete cost vector only to immediate neighbors; updates costs via $d_x(y) = \min_v \{c(x,v) + d_v(y)\}$.
- **Packet Switching**: Data broken into packets (Header + Payload) forwarded hop-by-hop using Store and Forward without prior resource reservation.
- **Addressing Hierarchy**:
  - **Physical Address (MAC)**: 48-bit (6-byte) link-local address changing hop-by-hop.
  - **Logical Address (IP)**: 32-bit (IPv4) or 128-bit (IPv6) globally unique end-to-end host identifier.
  - **Port Number**: 16-bit integer identifying process/application endpoints ($2^{16}=65536$ ports).
- **IPv4 vs. IPv6**:
  - IPv4 ($2^{32} \approx 4.29$B addresses) uses classful (A–E) and classless (CIDR `/n`) addressing.
  - IPv6 ($2^{128} \approx 3.4 \times 10^{38}$ addresses) uses 128-bit hexadecimal addressing with a streamlined fixed 40-byte base header.
- **DHCP**: Application-layer protocol dynamically allocating IP addresses, subnet masks, default gateways, and lease durations using discovery, offer, request, and renewal mechanisms.
