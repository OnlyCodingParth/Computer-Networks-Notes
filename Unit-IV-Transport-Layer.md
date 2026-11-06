# Unit IV – Transport Layer

## Table of Contents
- [1. Introduction to Transport Layer Services](#1-introduction-to-transport-layer-services)
  - [1.1 Role and Position in Network Architecture](#11-role-and-position-in-network-architecture)
  - [1.2 Services Offered by Transport Layer](#12-services-offered-by-transport-layer)
- [2. Multiplexing and Demultiplexing](#2-multiplexing-and-demultiplexing)
  - [2.1 Concepts, Sockets, and Port Numbers](#21-concepts-sockets-and-port-numbers)
  - [2.2 Multiplexing Process](#22-multiplexing-process)
  - [2.3 Demultiplexing Process](#23-demultiplexing-process)
- [3. Transmission Control Protocol (TCP)](#3-transmission-control-protocol-tcp)
  - [3.1 Overview and Key Characteristics](#31-overview-and-key-characteristics)
  - [3.2 High-Level Protocols Using TCP](#32-high-level-protocols-using-tcp)
  - [3.3 TCP Connection Management](#33-tcp-connection-management)
    - [3.3.1 Connection Establishment: 3-Way Handshake](#331-connection-establishment-3-way-handshake)
    - [3.3.2 Connection Termination Mechanisms](#332-connection-termination-mechanisms)
  - [3.4 TCP Segment Structure and Header Format](#34-tcp-segment-structure-and-header-format)
- [4. User Datagram Protocol (UDP)](#4-user-datagram-protocol-udp)
  - [4.1 Overview and Key Characteristics](#41-overview-and-key-characteristics)
  - [4.2 Suitable Real-Time Applications](#42-suitable-real-time-applications)
  - [4.3 UDP Segment Structure](#43-udp-segment-structure)
  - [4.4 UDP Checksum Calculation Mechanism](#44-udp-checksum-calculation-mechanism)
    - [4.4.1 Checksum Generation at Sender](#441-checksum-generation-at-sender)
    - [4.4.2 Step-by-Step Solved Numerical Example](#442-step-by-step-solved-numerical-example)
    - [4.4.3 Error Verification at Receiver](#443-error-verification-at-receiver)
- [5. Reliable Data Transfer (RDT) Protocols](#5-reliable-data-transfer-rdt-protocols)
  - [5.1 Fundamentals and Core Objectives](#51-fundamentals-and-core-objectives)
  - [5.2 Four Functions of Reliable Data Transfer](#52-four-functions-of-reliable-data-transfer)
  - [5.3 Flow Control and Sequence Number Management](#53-flow-control-and-sequence-number-management)
- [6. Pipelined Reliable Data Transfer Protocols](#6-pipelined-reliable-data-transfer-protocols)
  - [6.1 Concept of Pipelining and Link Utilization](#61-concept-of-pipelining-and-link-utilization)
  - [6.2 Transmission Error Scenarios in Pipelining](#62-transmission-error-scenarios-in-pipelining)
  - [6.3 Go-Back-N (GBN) Protocol](#63-go-back-n-gbn-protocol)
  - [6.4 Selective Repeat (SR) Protocol](#64-selective-repeat-sr-protocol)
- [7. General Connection-Oriented Transport Services](#7-general-connection-oriented-transport-services)
  - [7.1 Operational Sequence](#71-operational-sequence)
  - [7.2 Advantages and Disadvantages](#72-advantages-and-disadvantages)
- [8. Comparative Analysis Tables](#8-comparative-analysis-tables)
  - [8.1 TCP vs. UDP](#81-tcp-vs-udp)
  - [8.2 Go-Back-N (GBN) vs. Selective Repeat (SR)](#82-go-back-n-gbn-vs-selective-repeat-sr)
- [9. Important Technical Definitions](#9-important-technical-definitions)
- [10. Quick Reference: Important Diagrams and PDF Viewer Pages](#10-quick-reference-important-diagrams-and-pdf-viewer-pages)
- [11. Final Chapter Summary](#11-final-chapter-summary)

---

## 1. Introduction to Transport Layer Services

The **Transport Layer** operates directly above the Network Layer and below the Application Layer within the network protocol stack. Its primary role is to establish a **logical communication** channel directly between application processes running on distinct end systems (hosts).

```
+-------------------------------------------------------------+
|                      Application Layer                      |
|                  (Processes: P1, P2, P3...)                 |
+-------------------------------------------------------------+
                              |
                              v  (Transport Layer Interface)
+-------------------------------------------------------------+
|                       Transport Layer                       |
|           (Logical Process-to-Process Delivery)             |
+-------------------------------------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|                        Network Layer                        |
|            (Host-to-Host Datagram Delivery)                 |
+-------------------------------------------------------------+
```

### 1.1 Role and Position in Network Architecture

- **Direct Process Communication**: Unlike the Network Layer, which provides logical communication between physical hosts (host-to-host), the Transport Layer provides **process-to-process** communication. It allows user processes on different machines to exchange data as if they were directly connected.
- **Implementation at End Systems**: Transport layer protocols are implemented exclusively in the **end systems** (hosts) and not in intermediate network devices like routers. Routers only process protocols up to the Network Layer (Layer 3).
- **Two-Way Communication**: Applications can both read from and write to the transport layer, making transport layer communication an inherently bidirectional (two-way) process.
- **Multiple Protocols**: A computer network provides more than one protocol to application software. In the TCP/IP suite, the transport layer primarily offers two protocols: **Transmission Control Protocol (TCP)** and **User Datagram Protocol (UDP)**, each catering to different application requirements.

### 1.2 Services Offered by Transport Layer

The transport layer enhances the basic host-to-host delivery services of the network layer by offering the following core functions:

1. **End-to-End Connection between Hosts**:
   - Creates and manages logical end-to-end connections between communicating hosts.
   - Offers connection-oriented delivery with guarantees via **TCP** or connectionless best-effort delivery via **UDP**.
2. **Flow Control**:
   - Prevents a fast sender from overflowing the buffer of a slow receiver.
   - Employs a **sliding window protocol** where the receiver dynamically informs the sender of its available buffer capacity (window size).
3. **Multiplexing and Demultiplexing**:
   - Enables multiple application processes on a single host to concurrently share a single network connection and physical medium.
4. **Connection Establishment and Termination**:
   - Manages connection lifecycle using structured handshake algorithms before data exchange begins and graceful or abrupt teardown protocols when transmission completes.
5. **Reliable Data Delivery**:
   - Transforms the unreliable underlying network into an error-free channel by incorporating error detection mechanisms (checksums), sequence numbers, positive acknowledgements (**ACK**), negative acknowledgements (**NACK**), and automatic retransmissions.

**[📌 REFER TO PDF DIAGRAM: Multiplexing / Demultiplexing Overview – PDF PAGE NO. 2]**

---

## 2. Multiplexing and Demultiplexing

Multiplexing and demultiplexing are fundamental services provided by all transport layer protocols whenever a shared communication channel is utilized by multiple application processes.

### 2.1 Concepts, Sockets, and Port Numbers

- **Socket**: A **socket** is the intermediate interface or door through which an application process sends data to and receives data from the transport layer.
- **Port Number**: A 16-bit identifier assigned to specific application processes. A socket is uniquely identified by the combination of an IP address and a port number.
- **Socket Sharing**: A single machine running multiple applications (e.g., Web browser, Email client, FTP client) creates multiple sockets. Each process can potentially use many sockets.

### 2.2 Multiplexing Process

- **Definition**: **Multiplexing** (many-to-one) occurs at the sender side.
- **Mechanism**: The transport layer collects data units from multiple application processes via their respective sockets, encapsulates each data unit with a transport layer header containing **source port** and **destination port** addresses, assembles them into transport segments, and hands them down to the network layer as a single unified stream of packets.

### 2.3 Demultiplexing Process

- **Definition**: **Demultiplexing** (one-to-many) occurs at the receiver side.
- **Mechanism**: When the transport layer on the receiving machine receives a sequence of transport segments from the underlying network layer, it inspects the destination port number inside each segment's header and routes the segment to its corresponding application socket.

**[📌 REFER TO PDF DIAGRAM: Process-to-Socket Multiplexing and Demultiplexing Architecture – PDF PAGE NO. 9]**

---

## 3. Transmission Control Protocol (TCP)

**Transmission Control Protocol (TCP)** is a reliable, connection-oriented transport protocol that forms the backbone of internet communication.

### 3.1 Overview and Key Characteristics

- **Connection-Oriented**: TCP mandates that a logical connection be formally established between sender and receiver before any user data can be transmitted.
- **Reliable End-to-End Delivery**: Guarantees that data sent by a process reaches the destination completely, correctly, in correct sequence, and without duplication.
- **Positive Acknowledgement Re-transmission (PAR)**: TCP relies on PAR for reliability. When a sender transmits a segment, it sets a timer and expects a positive acknowledgement (**ACK**) from the receiver. If the ACK is not received before the timer expires, the sender automatically retransmits the segment.
- **Full-Duplex Communication**: Data can flow simultaneously in both directions between the client and server.

### 3.2 High-Level Protocols Using TCP

Because TCP ensures total reliability, error control, and in-order delivery, it is utilized by high-level application protocols where data loss is intolerable:

- **FTP (File Transfer Protocol)**: Uses TCP for reliable file transfers.
- **HTTP (HyperText Transfer Protocol)**: Uses TCP for loading web content accurately.
- **SMTP (Simple Mail Transfer Protocol)**: Uses TCP for reliable email transmission.

### 3.3 TCP Connection Management

TCP uses a client/server communication model. The **client** initiates the connection, while the **server** listens and responds.

#### 3.3.1 Connection Establishment: 3-Way Handshake

Connection management in TCP is performed via a **3-Way Handshake** procedure before user data transfer begins.

```
Client (Initiator)                                Server (Responder)
       |                                                  |
       |--------------- SYN (Seq = x) ------------------->|  Step 1: Connection Request
       |                                                  |
       |<-- SYN + ACK (Seq = y, Ack = x + 1) --------------|  Step 2: Request Ack & Server Init
       |                                                  |
       |--------------- ACK (Ack = y + 1) ---------------->|  Step 3: Final Confirmation
       |                                                  |
 [Connection Established]                           [Connection Established]
```

1. **Step 1: SYN (Synchronize)**
   - The client sends a TCP segment with the **SYN** control bit set to $1$ to a specific port on the server.
   - Serves as a formal **connection request**. It informs the server of the client's initial sequence number ($x$) to synchronize sequence numbers for subsequent data bytes.
2. **Step 2: SYN-ACK (Synchronize-Acknowledgement)**
   - Upon receiving the SYN packet, the listening server acknowledges the request by sending a segment with both **SYN** and **ACK** control bits set to $1$.
   - The ACK portion contains an acknowledgement number $x + 1$ (confirming receipt of client's SYN), while the SYN portion contains the server's own initial sequence number ($y$).
3. **Step 3: ACK (Acknowledgement)**
   - Upon receiving the SYN-ACK, the client responds with a segment where the **ACK** control bit is set to $1$ and acknowledgement number is $y + 1$.
   - Once this final handshake segment is received by the server, a full-duplex TCP connection is formally established, and both hosts are ready to exchange application data.

**[📌 REFER TO PDF DIAGRAM: TCP 3-Way Handshake Connection Establishment – PDF PAGE NO. 4]**

**[📌 REFER TO PDF DIAGRAM: Connection Management Handshake – PDF PAGE NO. 8]**

#### 3.3.2 Connection Termination Mechanisms

When data exchange is complete, a TCP connection can be closed using one of two mechanisms:

1. **Graceful Connection Release**:
   - Both communicating parties close their respective sides of the connection independently using the **FIN** flag.
   - When a host sets `FIN = 1`, it indicates it has no more data to send. The connection remains open in the opposite direction until the other party also sends a segment with `FIN = 1` and receives an ACK.
2. **Abrupt Connection Release**:
   - Either one TCP entity forces the connection closed immediately by setting the **RST** (Reset) flag, or a user terminates both directions of data transfer simultaneously.

### 3.4 TCP Segment Structure and Header Format

A TCP segment consists of a fixed **20-byte header** (which can be extended up to 60 bytes with optional fields), followed by application user data (up to 65,535 bytes total segment size).

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port (16 bits) |       Destination Port (16 bits)      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number (32 bits)                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Acknowledgement Number (32 bits)                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| HLEN | Resv |U|A|P|R|S|F|                    Window Size              |
|4 bits|6 bits|R|C|S|S|Y|I|                     (16 bits)               |
|      |      |G|K|H|T|N|N|                                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum (16 bits)    |          Urgent Pointer (16 bits)   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options and Padding (Up to 40 bytes)               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### Detailed Header Field Breakdown:

- **Source Port Address (16 bits)**: Identifies the sending application process port; used by receiver to send replies.
- **Destination Port Address (16 bits)**: Identifies the receiving application process port on the destination host.
- **Sequence Number (32 bits)**:
  - Specifies the byte sequence number of the first data byte in this segment.
  - During connection establishment (`SYN = 1`), if initial sequence number is $n$, the first data byte is numbered $n + 1$.
- **Acknowledgement Number (32 bits)**:
  - Valid if the `ACK` control flag is set to $1$.
  - Contains the sequence number of the **next expected byte** that the receiver is waiting to receive.
- **Data Offset / HLEN (4 bits)**: Specifies the length of the TCP header in 32-bit (4-byte) words. Indicates where user data begins.
- **Reserved (6 bits)**: Reserved for future use (set to zero).
- **Control Flags (6 bits)**:
  - **URG**: Urgent pointer field is valid.
  - **ACK**: Acknowledgement number field is valid.
  - **PSH**: Push function (requests immediate buffer delivery to receiving application).
  - **RST**: Reset the connection abruptly.
  - **SYN**: Synchronize sequence numbers during connection establishment.
  - **FIN**: Terminate the connection (no more data from sender).
- **Window Size (16 bits)**: Defines the sliding window size in bytes that the sender of this segment is willing to accept (used for flow control).
- **Checksum (16 bits)**: Provides mandatory 16-bit 1s complement error detection covering header and payload.
- **Urgent Pointer (16 bits)**: Valid only when `URG = 1`; specifies the offset from sequence number indicating location of urgent data bytes.
- **Options and Padding (Up to 40 bytes)**: Optional parameter space (e.g., Maximum Segment Size) padded with zeros to ensure header boundary aligns at 32 bits.

**[📌 REFER TO PDF DIAGRAM: TCP Segment Structure and Header Fields – PDF PAGE NO. 5]**

---

## 4. User Datagram Protocol (UDP)

**User Datagram Protocol (UDP)** is a lightweight, connectionless transport layer protocol that emphasizes simplicity, minimum delay, and efficiency over reliability.

### 4.1 Overview and Key Characteristics

- **Connectionless**: No handshake or connection setup takes place between sender and receiver prior to sending datagrams.
- **Unreliable / Best-Effort Delivery**: UDP provides no delivery confirmations, no acknowledgements, no automatic retransmissions, no flow control, and no congestion control.
- **Minimal Overhead**: Contains a minimal header size of only **8 bytes**, saving both network bandwidth and host processing latency.
- **No Sender/Receiver Waiting**: Packets are transmitted immediately as soon as they are handed down from the application layer.

### 4.2 Suitable Real-Time Applications

UDP is ideal for applications where low latency is critical and where losing occasional packets is preferable to experiencing transmission delays caused by retransmissions:

- **Online Multiplayer Gaming**: Requires instant state updates.
- **Voice over IP (VoIP) & Video Conferencing**: Real-time voice/video where delayed retransmitted frames are useless.
- **Live Video Streaming**: Prioritizes smooth continuous playout over packet loss.
- **DNS Queries**: Small request-response exchanges where connection setup overhead is unnecessary.

### 4.3 UDP Segment Structure

A UDP segment consists of a lightweight **8-byte header** followed by user data payload.

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port Number (16 bits)                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|       Destination Port Number (16 bits)                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Total Length (16 bits)                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|              Checksum (16 bits)                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             Data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### Field Descriptions:
1. **Source Port Number (16 bits)**: Identifies sending application process port.
2. **Destination Port Number (16 bits)**: Identifies application service on destination machine.
3. **Total Length (16 bits)**: Specifies the entire length of the UDP datagram (Header + Data) in bytes. Minimum value is $8$ bytes (header size alone).
4. **Checksum (16 bits)**: An optional 16-bit error detection field covering the entire UDP packet (header + data). If unused, all 16 bits are set to zeros.

> **Max Payload Calculation**:
> While UDP's total length field permits datagrams up to $65,535$ bytes, encapsulation in an IP packet limits actual maximum payload. Since the standard IP header takes $20$ bytes and UDP header takes $8$ bytes:
> $$\text{Max UDP Data Payload} = 65,535 - 20 - 8 = 65,507 \text{ bytes}$$

**[📌 REFER TO PDF DIAGRAM: UDP Segment Structure – PDF PAGE NO. 10]**

### 4.4 UDP Checksum Calculation Mechanism

UDP utilizes a **1s complement sum** of 16-bit words across the segment (header and payload) for error checking.

#### 4.4.1 Checksum Generation at Sender
1. The sender divides the entire UDP segment (along with a pseudo-header) into 16-bit binary words.
2. The 16-bit words are added together using 1s complement arithmetic.
3. Any carry overflow beyond 16 bits during addition is **wrapped around** (added back to the least significant bit).
4. The final sum is inverted (0s converted to 1s and 1s converted to 0s) to produce the **1s complement**, which is inserted into the 16-bit Checksum field.

#### 4.4.2 Step-by-Step Solved Numerical Example

**Problem Statement (From PDF)**:
Calculate the UDP Checksum for a segment given the following three 16-bit words:
- **Word 1**: `0110011001100000`
- **Word 2**: `0101010101010101`
- **Word 3**: `1000111100001100`

**Step-by-Step Solution**:

**Step 1: Add the first two 16-bit words**
$$\begin{array}{r@{\quad}l}
  \texttt{0110011001100000} & \text{(Word 1)} \\
+ \;\mathbf{\texttt{0101010101010101}} & \text{(Word 2)} \\
\hline
  \mathbf{\texttt{1011101110110101}} & \text{(Sum 1)}
\end{array}$$

**Step 2: Add the third 16-bit word to the previous sum**
$$\begin{array}{r@{\quad}l}
  \texttt{1011101110110101} & \text{(Sum 1)} \\
+ \;\mathbf{\texttt{1000111100001100}} & \text{(Word 3)} \\
\hline
  \mathbf{\texttt{10100101011000010}} & \text{(17-bit Intermediate Result with Carry Overflow)}
\end{array}$$

**Step 3: Wrap around the overflow carry bit**
The addition resulted in a 17-bit number with a leading carry overflow bit (`1`). Wrap this carry bit around by adding it back to the lower 16 bits:
$$\begin{array}{r@{\quad}l}
  \texttt{0100101011000010} & \text{(Lower 16 bits)} \\
+ \;\mathbf{\texttt{0000000000000001}} & \text{(Wrapped Carry Bit)} \\
\hline
  \mathbf{\texttt{0100101011000011}} & \text{(Final Sum)}
\end{array}$$

**Step 4: Take the 1s complement of the Final Sum**
Invert all bits ($0 \to 1$ and $1 \to 0$):
$$\begin{array}{r@{\quad}l}
  \text{Final Sum:} & \texttt{0100101011000011} \\
  \mathbf{\text{1s Complement (Checksum):}} & \mathbf{\texttt{1011010100111100}}
\end{array}$$
*(Note: As recorded in PDF Page 11, taking 1s complement of `0100101011000010` gives `1011010100111101` when calculated with wrapped carry).*

#### 4.4.3 Error Verification at Receiver
When the receiver obtains the packet:
1. It adds all 16-bit words received, including the transmitted **Checksum word**.
2. If no errors occurred during transmission, the resulting 1s complement sum at the receiver will equal **all 1s** (`1111111111111111`).
3. If any bit in the sum is $0$, the receiver knows an error occurred and discards the segment.

---

## 5. Reliable Data Transfer (RDT) Protocols

Reliable Data Transfer (RDT) protocols employ algorithmic controls at the transport layer to guarantee error-free, loss-free, and ordered delivery over physically unreliable network channels.

### 5.1 Fundamentals and Core Objectives

Errors during packet transmission generally occur in physical network hardware during buffering, propagation, or transmission actions. RDT protocol design must solve two primary concerns:
1. **Data Loss**: Packets dropped by overloaded router queues or line drops.
2. **Data Corruption**: Bit errors introduced by channel noise.

Solving loss and corruption introduces a third concern:
3. **Duplicate Data**: Generated when retransmissions occur due to delayed or lost acknowledgements.

### 5.2 Four Functions of Reliable Data Transfer

Implementing RDT involves four fundamental checkpoints/actions:
1. **Receive Data**: Accept data from application layer or incoming network frame.
2. **Encapsulate Data Prior to Sending**: Add transport headers, sequence numbers, and checksums.
3. **Un-encapsulate Data upon Receipt**: Strip transport headers and verify integrity.
4. **Send Data**: Pass data packets to underlying network layer or upper application process.

### 5.3 Flow Control and Sequence Number Management

- **Sequence Numbers**: TCP assigns sequence numbers to segments. The sender tracks which segments have been transmitted and acknowledged.
- **Acknowledgement (ACK) and NACK**: Receiver sends ACK for correct packets and NACK (or duplicate ACK) for missing/corrupted packets.
- **Discarding Out-of-Order Packets**: If an incoming segment's sequence number does not match expected sequence number, receiver discards it and re-issues NACK/ACK for expected packet.
- **TCP Timestamps**: If two segments arrive with identical sequence numbers, TCP compares timestamp values to discard older duplicate packets.

---

## 6. Pipelined Reliable Data Transfer Protocols

Stop-and-Wait protocols suffer from extremely poor link utilization because the sender must wait for an ACK before transmitting the next packet. **Pipelining** resolves this inefficiency.

### 6.1 Concept of Pipelining and Link Utilization

In a **pipelined reliable data transfer protocol**, the sender is allowed to transmit multiple data packets continuously without waiting for an immediate acknowledgement for each packet.

```
Stop-and-Wait Transmission:
Sender:   |-- Pkt 0 -->| (Wait ACK) |<-- ACK 0 --|-- Pkt 1 -->| (Low Link Utilization)

Pipelined Transmission:
Sender:   |-- Pkt 0 -->|-- Pkt 1 -->|-- Pkt 2 -->|-- Pkt 3 -->| (High Link Utilization)
```

- **Benefit**: Significantly reduces total transmission time and maximizes link bandwidth utilization.

### 6.2 Transmission Error Scenarios in Pipelining

When pipelining is used, two primary error scenarios must be handled:
1. **Lost or Corrupted Data Packet**: A packet fails to reach the destination.
2. **Lost or Corrupted Acknowledgement Packet**: Receiver's ACK is lost en route to sender.

Two primary sliding-window protocol approaches exist to solve these scenarios: **Go-Back-N** and **Selective Repeat**.

### 6.3 Go-Back-N (GBN) Protocol

In **Go-Back-N (GBN)**, the sender is allowed to transmit up to $N$ unacknowledged packets (the sliding window size) in the pipeline.

```
Sender Window (Size N = 4):
[0  1  2  3] 4  5  6  7 ...
  |     |
  |     +-- Last packet sent
  +-------- First unacknowledged packet (Base)
```

#### Key Operational Features of GBN:
1. **Cumulative Acknowledgements**: Receiver sends a cumulative ACK $k$, indicating that all packets up to and including sequence number $k$ have been correctly received.
2. **Receiver Simplicity**: The receiver accepts packets **only in strict sequential order**.
3. **Discarding Out-of-Order Packets**: If packet $k$ is lost, any subsequent out-of-order packets ($k+1, k+2, \dots$) received are **discarded** by the receiver, and the receiver re-sends ACK for the last correctly received in-order packet.
4. **Timeout Retransmission**: The sender maintains a single timer for the oldest unacknowledged packet. If a timeout occurs for packet $k$, the sender **retransmits ALL unacknowledged packets** sent since $k$ (i.e., it "goes back N" packets).

**[📌 REFER TO PDF DIAGRAM: Go-Back-N Protocol Operation Flow – PDF PAGE NO. 13]**

### 6.4 Selective Repeat (SR) Protocol

**Selective Repeat (SR)** overcomes GBN's unnecessary retransmissions by retransmitting **only those specific packets** that are actually lost or corrupted.

```
SR Sender/Receiver Buffering:
Sender:   [0  1  2  3]  (Pkt 2 lost) -> Retransmits ONLY Pkt 2 upon timeout
Receiver: [0  1  _  3]  (Buffers Pkt 3 while waiting for retransmitted Pkt 2)
```

#### Key Operational Features of SR:
1. **Individual Acknowledgements**: The receiver sends individual ACKs for every correctly received packet, regardless of whether it arrives in order.
2. **Receiver Buffering**: The receiver contains a receive buffer equal to window size $N$. Out-of-order packets are **buffered** until missing lower-sequence packets arrive.
3. **Batch Delivery**: Once missing packets arrive, a complete sequential batch of packets is delivered to the application layer.
4. **Individual Timers**: The sender maintains logical individual timers for each unacknowledged packet and retransmits only the single timed-out packet.

**[📌 REFER TO PDF DIAGRAM: Selective Repeat Protocol Operations Flow – PDF PAGE NO. 14]**

---

## 7. General Connection-Oriented Transport Services

Connection-oriented transport provides a reliable, ordered communication stream between processes, mirroring session-layer concepts.

### 7.1 Operational Sequence
A connection-oriented protocol follows a strict 3-phase operational sequence:
1. **Establishing Connection**: A logical session connection is initialized between hosts prior to data transfer.
2. **Transferring Data or Message**: Data stream is divided into smaller segments and transmitted across established connection.
3. **Releasing Connection**: Connection resources are formally released after data transfer terminates.

### 7.2 Advantages and Disadvantages

#### Advantages:
- **Quality of Service (QoS)**: Easy support for QoS guarantees.
- **High Reliability**: Significantly more reliable than connectionless services.
- **Packet Division**: Large application messages are divided into smaller, manageable network packets.
- **Reduced Duplicates**: Duplicate packet issues are minimized through systematic sequencing.

#### Disadvantages:
- **Fixed Overhead Cost**: Fixed setup and maintenance overhead regardless of actual traffic volume.
- **Mandatory Resource Allocation**: Requires pre-allocation of buffers and state tables prior to communication.
- **No Alternative Routing**: Sensitive to path failures or network congestion, as all segments follow the established connection path without dynamic rerouting.

---

## 8. Comparative Analysis Tables

### 8.1 TCP vs. UDP

| Feature / Dimension | Transmission Control Protocol (TCP) | User Datagram Protocol (UDP) |
| :--- | :--- | :--- |
| **Connection Type** | Connection-Oriented (Requires 3-way handshake) | Connectionless (No handshake) |
| **Reliability** | Reliable (Guaranteed delivery via PAR & ACKs) | Unreliable (Best-effort delivery, no ACKs) |
| **Header Size** | Fixed 20 bytes (Up to 60 bytes with Options) | Fixed 8 bytes |
| **Data Flow & Speed** | Slower (Overhead due to error/flow control) | Faster (Minimal latency and overhead) |
| **Flow & Congestion Control**| Supports Flow Control (Sliding Window) & Congestion Control | No Flow Control or Congestion Control |
| **Ordering** | In-order delivery guaranteed (Sequence Numbers) | No ordering guaranteed (Datagrams may arrive out-of-order) |
| **Error Checking** | Mandatory Checksum + Retransmissions | Optional Checksum, no retransmissions |
| **Primary Applications** | Web (HTTP), Email (SMTP), File Transfer (FTP) | Real-time gaming, VoIP, Live Streaming, DNS |

### 8.2 Go-Back-N (GBN) vs. Selective Repeat (SR)

| Parameter / Feature | Go-Back-N (GBN) ARQ | Selective Repeat (SR) ARQ |
| :--- | :--- | :--- |
| **Retransmission Scope** | Retransmits packet $k$ AND all subsequent unacknowledged packets | Retransmits ONLY the single lost/corrupted packet $k$ |
| **Acknowledgement Type** | Cumulative ACK | Individual ACK for each packet |
| **Receiver Buffering** | No receiver buffering (Discards out-of-order packets) | Receiver buffers out-of-order packets |
| **Sender Timers** | Single timer for oldest unacknowledged packet | Individual timers for each unacknowledged packet |
| **Bandwidth Efficiency** | Lower (Unnecessary retransmission of valid packets) | Higher (Only lost packets resent) |
| **Implementation Complexity**| Simple sender and receiver logic | Complex buffering and timer management |

---

## 9. Important Technical Definitions

> 1. **Process-to-Process Communication**: Logical transport layer communication directly between application processes running on different host computers.
> 2. **Socket**: An operating system interface combining IP address and 16-bit Port Number through which an application process communicates with transport layer protocols.
> 3. **Multiplexing**: The transport layer process of collecting data from multiple application sockets, adding headers, and combining them into a single packet stream sent to the network layer.
> 4. **Demultiplexing**: The transport layer process of receiving a segment stream from the network layer and routing each segment to its specific destination socket based on destination port numbers.
> 5. **Positive Acknowledgement Re-transmission (PAR)**: A reliability mechanism where the sender retransmits data unless a positive confirmation (ACK) is received before timer expiration.
> 6. **TCP 3-Way Handshake**: A 3-step synchronization procedure (SYN $\to$ SYN-ACK $\to$ ACK) used by TCP to establish a full-duplex connection between client and server.
> 7. **1s Complement Checksum**: An error detection code calculated by summing 16-bit binary words with carry wrapping and inverting the bits.
> 8. **Pipelining**: A performance technique allowing a sender to transmit multiple data packets prior to receiving an acknowledgement for the first packet.
> 9. **Go-Back-N (GBN) ARQ**: A pipelined protocol where packet loss triggers retransmission of the lost packet and all subsequent sent packets.
> 10. **Selective Repeat (SR) ARQ**: A pipelined protocol where the receiver buffers out-of-order packets and the sender retransmits only lost packets.

---

## 10. Quick Reference: Important Diagrams and PDF Viewer Pages

| Diagram Name | Description / Topic | PDF Page No. |
| :--- | :--- | :--- |
| **Multiplexing / Demultiplexing Overview** | High-level data flow between app processes and transport layer | Page 2 |
| **TCP 3-Way Handshake Connection Establishment** | SYN, SYN-ACK, ACK sequence flow between Client and Server | Page 4 |
| **TCP Segment Structure and Header Fields** | 20-60 Byte TCP header format breakdown | Page 5 |
| **Connection Management Handshake** | Initiator to Responder handshake sequence diagram | Page 8 |
| **Process-to-Socket Multiplexing Architecture** | Application processes $P_1, P_2, P_3$ mapping to Sockets | Page 9 |
| **UDP Segment Structure** | 8-Byte UDP header field structure | Page 10 |
| **Go-Back-N Protocol Operation Flow** | GBN packet loss, discard, cumulative ACK, and timeout retransmissions | Page 13 |
| **Selective Repeat Protocol Operations Flow** | SR individual ACKs, out-of-order buffering, and selective retransmission | Page 14 |

---

## 11. Final Chapter Summary

Unit IV provides a complete breakdown of the **Transport Layer**, bridging application processes with underlying host-to-host network delivery. Key takeaways include:

- **Protocols**: **TCP** provides reliable, connection-oriented, flow-controlled, in-order byte-stream delivery via a 3-way handshake and 20-60 byte header format. **UDP** provides low-overhead (8-byte header), connectionless best-effort delivery ideal for real-time applications.
- **Multiplexing/Demultiplexing**: Utilizes 16-bit port numbers and sockets to direct concurrent application data streams over shared network media.
- **Reliable Data Transfer**: Uses 1s complement checksums, sequence numbers, ACKs/NACKs, and timers to counter data loss, corruption, and duplication.
- **Pipelining Protocols**: **Go-Back-N (GBN)** uses cumulative ACKs and retransmits all packets from a loss point without receiver buffering. **Selective Repeat (SR)** uses individual ACKs and receiver buffering to retransmit only lost packets, maximizing bandwidth efficiency.
