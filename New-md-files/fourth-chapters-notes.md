# CHAPTER 4: UNIT IV – TRANSPORT LAYER

---

## Module 1: Introduction & Foundational Concepts

In computer networking, the transport layer provides logical, process-to-process communication between applications running on different hosts. When multiple application processes run concurrently on a single machine, **multiplexing** and **demultiplexing** are necessary whenever a communication channel is shared:

- **Socket:** The intermediate interface or door through which an application process sends and receives data to/from the transport layer. A single machine running several applications creates multiple sockets.
- **Port Number:** A **16-bit identifier** assigned to uniquely identify a specific application process. A socket is uniquely addressed using a combination of an **IP address** and a **Port number**.

### 2. Multiplexing (Sender Side: Many-to-One)

- **Definition:** Multiplexing is the process where data is collected from multiple application processes, assembled into segments with appropriate headers, and passed to the network layer as a single data stream.
- **Mechanism:**
    1. The transport layer collects chunks of data from multiple processes via their respective sockets.
    2. It encapsulates each data chunk with a transport-layer header containing both the **Source Port Number** and the **Destination Port Number**.
    3. The assembled transport segments are handed down to the Network Layer to be transmitted across the physical medium.

### 3. Demultiplexing (Receiver Side: One-to-Many)

- **Definition:** Demultiplexing is the process of receiving a sequence of transport segments from the network layer and delivering the payload directly to the correct application socket.
- **Mechanism:**
    1. When transport segments arrive at the receiving host from the underlying Network Layer, the transport layer inspects the segment header.
    2. It specifically checks the **Destination Port Number**.
    3. It routes and delivers each data segment to the corresponding application socket running on the host.

### 4. Exam Diagram: Multiplexing and Demultiplexing Architecture

*(Draw this neat block diagram in the exam book)*

```text
         SENDER HOST                                      RECEIVER HOST
+---------------------------+                     +---------------------------+
|   Application Processes   |                     |   Application Processes   |
| [Yahoo] [Gmail] [Outlook] |                     | [Yahoo] [Gmail] [Outlook] |
+---------------------------+                     +---------------------------+
     |       |        |                                ^       ^        ^
     v       v        v                                |       |        |
+----+-------+--------+-----+                     +----+-------+--------+-----+
|      Sockets (Ports)      |                     |      Sockets (Ports)      |
+----+-------+--------+-----+                     +----+-------+--------+-----+
     \       |        /                                ^       ^        ^
       \     |      /                                    \     |      /
     [ MULTIPLEXING ]                                  [ DEMULTIPLEXING ]
       (Many-to-One)                                     (One-to-Many)
              |                                                 ^
              v                                                 |
+---------------------------+                     +---------------------------+
|      Transport Layer      |                     |      Transport Layer      |
+-------------+-------------+                     +-------------+-------------+
              |                                                 ^
              v                                                 |
       [Network Layer] ======= (Physical Medium) ======> [Network Layer]
```

### 5. Comparative Analysis Table

| Parameter / Feature | Multiplexing | Demultiplexing |
| --- | --- | --- |
| **Operational Direction** | Many-to-One (combines multiple process streams into one). | One-to-Many (distributes single stream to multiple processes). |
| **Location of Execution** | Occurs at the Sender / Source host. | Occurs at the Receiver / Destination host. |
| **Primary Function** | Gathers data units from sockets, adds transport headers (source/dest ports), and forms segments. | Receives incoming segments, separates them, and forwards data to respective sockets. |
| **Header Processing** | Adds/Inserts both Source Port and Destination Port fields. | Inspects/Evaluates the Destination Port field. |
| **Data Hand-off** | Hands data down from Application Layer to Network Layer. | Hands data up from Network Layer to Application Layer. |

---

## Module 2: Transmission Control Protocol (TCP) & Connection Management

### A. Introduction to TCP Connection Establishment

- **Transmission Control Protocol (TCP)** is a connection-oriented, reliable transport layer protocol.
- Before any application data can be transmitted, TCP mandates that a logical end-to-end full-duplex connection be formally established between the **Client (Initiator)** and the **Server (Responder)**.
- This setup procedure is executed using a **3-Way Handshake** (commonly known as **SYN** → **SYN+ACK** → **ACK**), which synchronizes sequence numbers between both endpoints.

### B. The 3-Way Handshake Sequence (Step-by-Step)

1. **Step 1: SYN (Connection Request from Client)**
    - The client initiates the connection by transmitting a TCP segment with the **SYN control bit set to 1** to a specified port on the server.
    - It specifies an initial sequence number ($Seq = x$).
    - **Purpose:** Acts as a formal request to synchronize sequence numbers and establish a communication channel.
2. **Step 2: SYN + ACK (Server Response & Acknowledgement)**
    - Upon receiving the connection request, if the server agrees to connect, it responds with a segment having both **SYN and ACK bits set to 1**.
    - It sets:
        - **Acknowledgment Number:** $Ack = x + 1$ (acknowledging receipt of the client's `SYN`).
        - **Sequence Number:** $Seq = y$ (the server's own initial sequence number).
    - **Purpose:** Acknowledges the client's request and initiates synchronization from the server's side.
3. **Step 3: ACK (Client Confirmation)**
    - Upon receiving the `SYN + ACK`, the client transmits a final segment with the **ACK bit set to 1**.
    - It sets the **Acknowledgment Number:** $Ack = y + 1$ (confirming receipt of the server's sequence number).
    - **Outcome:** With this third step complete, a reliable, full-duplex TCP connection is established, and both devices begin normal data exchange.

### C. Standard Exam Handshake Diagram

```text
   Client (Initiator)                                 Server (Responder)
            |                                                  |
            |--------------- SYN (Seq = x) ------------------->|  Step 1: Connection Request
            |                                                  |
            |<-- SYN + ACK (Seq = y, Ack = x + 1) -------------|  Step 2: Request Ack & Server Init
            |                                                  |
            |--------------- ACK (Ack = y + 1) --------------->|  Step 3: Final Confirmation
            |                                                  |
      [ESTABLISHED]                                      [ESTABLISHED]
            |                                                  |
            |<========== Reliable Full-Duplex Data ===========>|
```

### D. Connection Termination (Release)

1. **Graceful Release:** Initiated by sending a segment with the **FIN bit set to 1**. Each direction closes independently, ensuring no in-transit data is lost.
2. **Abrupt Release:** Forced immediately by setting the **RST (Reset) bit to 1**, which instantly terminates the connection in both directions.

### E. TCP Segment Structure

- A TCP segment is composed of a **header** followed by application layer **data**.
- **Header Size:** The minimum/standard header length is **20 bytes** (fixed part). It can extend up to **60 bytes** when optional fields are utilized (up to 40 bytes of options).
- **Data Unit:** While UDP transmits independent packets (datagrams), TCP views data as an unstructured, ordered stream of bytes segmented into blocks.

### 2. Standard Examination Header Format Diagram (32-Bit Grid)

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Source Port (16 bits)     |   Destination Port (16 bits)  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                   Sequence Number (32 bits)                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                Acknowledgement Number (32 bits)               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  HLEN |    Resv   |U|A|P|R|S|F|                               |
| 4 bits|   6 bits  |R|C|S|S|Y|I|     Window Size (16 bits)     |
|       |           |G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|       Checksum (16 bits)      |    Urgent Pointer (16 bits)   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|              Options and Padding (0 to 40 bytes)              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                              DATA                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### 3. Detailed Explanation of Header Fields

#### 1. Source Port Address (16 bits)

- Identifies the port number of the application process running on the sending host.
- Enables the receiving host to route replies back to the sender process.

#### 2. Destination Port Address (16 bits)

- Identifies the port number of the receiving application process on the destination host (used for demultiplexing).

#### 3. Sequence Number (32 bits)

- Identifies the byte number of the first data byte carried in this segment.
- If `SYN = 1` during connection establishment, this contains the **Initial Sequence Number (ISN)** $n$, and the first payload data byte will be numbered $n + 1$.

#### 4. Acknowledgement Number (32 bits)

- Valid only when the **ACK control bit is set to 1**.
- Contains the sequence number of the **next expected byte** that the receiver is waiting to receive. It acts as a cumulative acknowledgment (meaning all previous bytes have been received successfully).

#### 5. Header Length (HLEN / Data Offset) (4 bits)

- Indicates the total size of the TCP header measured in **32-bit (4-byte) words**.
- Since the minimum header is 20 bytes, the minimum value is $20 / 4 = 5$ (`0101`). The maximum is $60 / 4 = 15$ (`1111`).
- Tells the receiver where the TCP header ends and user payload data begins.

#### 6. Reserved (6 bits)

- Reserved for future protocol enhancements (always set to zero).

#### 7. Control Flags (6 single-bit flags)

Each bit controls specific signaling in TCP connection management:

- **URG (Urgent):** Indicates that the *Urgent Pointer* field is valid and that urgent data needs prompt processing.
- **ACK (Acknowledgment):** Indicates that the *Acknowledgement Number* field is valid.
- **PSH (Push):** Instructs the receiver to push the buffered data immediately up to the receiving application without waiting for buffers to fill.
- **RST (Reset):** Abruptly resets and tears down a broken or invalid connection.
- **SYN (Synchronize):** Used during connection setup to synchronize sequence numbers between sender and receiver.
- **FIN (Finish):** Indicates that the sender has finished transmitting data and requests a graceful connection termination.

#### 8. Window Size (16 bits)

- Implements **Flow Control**.
- Specifies the number of bytes that the receiver's available buffer can currently accept. This prevents a fast sender from overwhelming a slower receiver.

#### 9. Checksum (16 bits)

- Provides mandatory 16-bit 1s complement error detection covering the TCP pseudo-header, TCP header, and segment data payload.

#### 10. Urgent Pointer (16 bits)

- Valid only when the **URG bit is set to 1**.
- Points to the offset from the current sequence number where urgent data ends.

#### 11. Options & Padding (Up to 40 bytes)

- Used for optional features such as defining the **MSS (Maximum Segment Size)** or Window Scaling. Padding with zeros ensures the total header length is an exact multiple of 4 bytes (32 bits).

---

## Module 3: User Datagram Protocol (UDP) & Checksum Calculation

### 1. Overview & Key Characteristics of UDP

- **Connectionless Service:** Does not establish a logical connection or perform a handshake prior to sending datagrams. Packets are transmitted immediately.
- **Best-Effort / Unreliable Delivery:** UDP provides no delivery guarantees, acknowledgments, retransmissions, flow control, or congestion control.
- **Minimal Overhead:** Has a fixed, lightweight header of only **8 bytes** (compared to TCP's 20–60 bytes), which saves bandwidth and minimizes latency.
- **Suitable Applications:** Real-time applications where low delay is prioritized over lost packets (e.g., DNS queries, Voice over IP / VoIP, video conferencing, live streaming, online gaming).

### 3. Ideal Exam Answer: UDP Segment Structure

**Question:** *Illustrate UDP segment structure.* **[4 Marks]**

#### A. Overview

A UDP segment consists of an **8-byte header** divided into four 16-bit (2-byte) fields, followed by the application payload data.

#### B. Diagram: UDP Header Format (32-Bit Grid)

*(Draw this diagram in the exam book, from Page 10 of your PDF / Section 4.3):*

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|       Source Port Number      |    Destination Port Number    |
|           (16 bits)           |           (16 bits)           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Total Length (16 bits)    |       Checksum (16 bits)      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                              DATA                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### C. Description of Header Fields

1. **Source Port Number (16 bits):** Identifies the port number of the application process running on the sending machine.
2. **Destination Port Number (16 bits):** Identifies the target application service on the receiving host (used for demultiplexing).
3. **Total Length (16 bits):** Specifies the total length of the UDP segment (**Header + Data**) in bytes. Its minimum possible value is **8 bytes** (when data payload is empty).
4. **Checksum (16 bits):** An error-detection field covering the pseudo-header, UDP header, and payload data using 1s complement arithmetic.
5. **Maximum Data Payload Calculation:**

$$\text{Max UDP Data Payload} = 65{,}535 \text{ bytes (IP Max)} - 20 \text{ bytes (IP Header)} - 8 \text{ bytes (UDP Header)} = 65{,}507 \text{ bytes}$$

---

## Module 4: Pipelined Reliable Data Transfer

### Part 1: Ideal Examination Answer for Go-Back-N (GBN) Protocol

#### 1. Introduction & Pipelining Concept

- In Stop-and-Wait protocols, the sender must wait for an acknowledgement (ACK) before transmitting the next packet, resulting in very low link utilisation.
- **Go-Back-N (GBN)** is a **pipelined sliding-window protocol** that allows the sender to transmit multiple data packets without waiting for an individual ACK for each packet, subject to a maximum allowable window size of $N$ unacknowledged packets in the pipeline.

#### 2. Working Mechanism of GBN

- **Sender Window (Size $N$):** The sender can transmit up to $N$ consecutive packets before blocking. As acknowledgements arrive, the window slides forward over the sequence number space.
- **Cumulative Acknowledgements:** The receiver uses **cumulative ACKs**. When the receiver sends $ACK(k)$, it confirms that all packets up to and including sequence number $k$ have been received correctly and in order.
- **No Receiver Buffering:** The receiver accepts packets **only in strict sequential order**. It maintains no buffer for out-of-order packets; any packet that arrives out of order is **discarded immediately**, and the receiver re-sends an ACK for the last correctly received in-order packet.
- **Single Timer at Sender:** The sender maintains a **single timer** for the oldest unacknowledged packet.

#### 3. Error Handling & Packet Loss Scenario

- If a packet (e.g., packet $k$) is lost or corrupted in transit:
    1. The receiver discards all subsequent arriving packets ($k+1, k+2, \dots$) because they are out of order.
    2. The receiver repeatedly sends duplicate ACKs for the last in-order packet received.
    3. When the sender's timer for packet $k$ expires, the sender must **"go back" and retransmit packet $k$ AND all subsequent unacknowledged packets** in the window, even if the receiver had already seen them.

#### 4. Standard Exam Diagram: Go-Back-N in Operation

```text
SENDER                              RECEIVER
   |                                    |
   |---------- send pkt0 -------------->| rcv pkt0, send ACK0
   |---------- send pkt1 -------------->| rcv pkt1, send ACK1
   |---------- send pkt2 (LOSS) ------->X (packet lost)
   |---------- send pkt3 -------------->| rcv pkt3, DISCARD, send ACK1
   |                                    |
   |<--------- rcv ACK0 ----------------|
   |---------- send pkt4 -------------->| rcv pkt4, DISCARD, send ACK1
   |<--------- rcv ACK1 ----------------|
   |---------- send pkt5 -------------->| rcv pkt5, DISCARD, send ACK1
   |                                    |
   |           [pkt2 timeout]           |
   |                                    |
   |---------- send pkt2 -------------->| rcv pkt2, deliver, send ACK2
   |---------- send pkt3 -------------->| rcv pkt3, deliver, send ACK3
   |---------- send pkt4 -------------->|
   |---------- send pkt5 -------------->|
```

---

### Part 2: Ideal Examination Answer for Selective Repeat (SR) Protocol

#### 1. Introduction & Core Objective

- The **Selective Repeat (SR)** protocol is designed to avoid the unnecessary retransmissions of Go-Back-N by having the sender retransmit **only those specific packets that are suspected of being lost or corrupted**.
- Both the sender and the receiver maintain a window of size $N$ to manage outstanding packets and acceptable sequence numbers.

#### 2. Working Mechanism of SR

- **Individual Acknowledgements:** Unlike GBN's cumulative ACKs, the SR receiver transmits an **individual ACK for every correctly received packet**, regardless of whether it arrives in order or out of order.
- **Receiver Buffering:** The receiver is equipped with a buffer. When out-of-order packets arrive, the receiver accepts and **buffers them temporarily** while waiting for the missing lower-sequence packets to arrive.
- **Batch In-Order Delivery:** Once the missing packet is finally received, the receiver delivers the entire consecutive batch of buffered packets in order to the upper application layer.
- **Independent Timers at Sender:** The sender maintains **individual logical timers** for each transmitted, unacknowledged packet. A timeout triggers the retransmission of **only that single lost packet**.

#### 3. Error Handling & Retransmission Scenario

- When packet 2 is lost in transit:
    1. The receiver receives packets 3, 4, etc., stores them in its **receive buffer**, and immediately replies with `ACK3`, `ACK4`, etc.
    2. The sender marks packets 3 and 4 as acknowledged, but its timer for packet 2 continues running.
    3. Upon **timeout of packet 2**, the sender retransmits **only packet 2**.
    4. Upon receiving the retransmitted packet 2, the receiver combines it with the buffered packets (3, 4) and delivers packets 2, 3, and 4 to the application layer in one batch.

#### 4. Standard Exam Diagram: Selective Repeat Operations

```text
SENDER                              RECEIVER
   |                                    |
   |---------- send pkt0 -------------->| rcv pkt0, deliver, send ACK0
   |---------- send pkt1 -------------->| rcv pkt1, deliver, send ACK1
   |---------- send pkt2 (LOSS) ------->X (packet lost)
   |---------- send pkt3 -------------->| rcv pkt3, BUFFER, send ACK3
   |                                    |
   |<--------- rcv ACK0 ----------------|
   |---------- send pkt4 -------------->| rcv pkt4, BUFFER, send ACK4
   |                                    |
   |           [pkt2 timeout]           |
   |                                    |
   |---------- RESEND pkt2 ------------>| rcv pkt2, DELIVER (pkts 2,3,4)
   |                                    | send ACK2
   |<--------- rcv ACK1 ----------------|
   |---------- send pkt5 -------------->| rcv pkt5, deliver, send ACK5
```

---

### Part 3: Comparative Analysis Table (Difference Between GBN and SR)

| Parameter / Dimension | Go-Back-N (GBN) Protocol | Selective Repeat (SR) Protocol |
| --- | --- | --- |
| **Retransmission Scope** | Retransmits the lost packet **AND all subsequent unacknowledged packets** in the window. | Retransmits **ONLY the single lost or corrupted packet**. |
| **Acknowledgement Type** | **Cumulative ACKs** (e.g., $ACK(n)$ acknowledges all packets up to $n$). | **Individual ACKs** sent for each correctly received packet. |
| **Receiver Buffering** | **No receiver buffer**; out-of-order packets are discarded. | **Receiver maintains a buffer** to store out-of-order packets. |
| **Sender Timers** | Uses a **single timer** for the oldest unacknowledged packet. | Uses **independent individual timers** for each unacknowledged packet. |
| **Bandwidth Efficiency** | **Lower**, due to redundant retransmission of already received packets. | **Higher**, because only missing packets consume retransmission bandwidth. |
| **Implementation Complexity** | **Simpler logic** at both sender and receiver. | **More complex**, due to packet buffering and multiple timer management. |
| **Out-of-Order Handling** | Discards out-of-order packets and requests retransmission. | Stores out-of-order packets until missing packets arrive, then delivers in batch. |

---

## Editor's Verification Notes

These items were left as in your teacher's notes or adjusted only in layout. Please confirm them against the original reference notes.

1. **[DIAGRAM NEEDS VERIFICATION] UDP header diagram (Module 3, B):** The pasted version drew each of the four 16-bit fields on its own full-width row. I rearranged it to two fields per 32-bit row (Source/Destination Port, then Length/Checksum), matching the TCP grid. If your teacher's diagram uses one field per row, restore that layout.
2. **[DIAGRAM NEEDS VERIFICATION] `[pkt2 timeout]` marker (GBN and SR diagrams):** The pasted text lost its line breaks, so the marker's exact position was unclear. I placed it on its own line between the two lifelines, immediately before the retransmission of `pkt2`.
3. **[DIAGRAM NEEDS VERIFICATION] GBN diagram, last two lines:** After `ACK3` the sender resends `pkt4` and `pkt5` with no receiver annotation, exactly as in the pasted text.
4. **Module 1, Data Hand-off row:** The notes say multiplexing hands data "from Application Layer to Network Layer". Strictly, the transport layer hands segments down to the Network Layer. Kept as written.
5. **Numbering:**
    - Module 1 has no section "1" (the introduction sits directly under the module title; numbering begins at 2).
    - Module 2 numbering restarts at "2." and "3." after section E.
    - Module 3 has sections "1." and "3." with no "2." in the supplied notes, so a section may be missing. Please check.
6. **Removed copy-paste artifacts:** citation markers such as `[1]`, `[8][9]`, stray digit suffixes (e.g. `28`, `910`, `67`, `712`), the text `more_horiz`, HTML entities (`&gt;`, `&amp;`, `&#32;`), doubled backslashes in diagrams, and the broken table fragments inside the TCP and Multiplexing diagrams.
