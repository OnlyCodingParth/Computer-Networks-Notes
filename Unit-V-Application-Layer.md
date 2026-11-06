# Unit V – Application Layer

## Table of Contents
1. [Introduction to Application Layer Services](#1-introduction-to-application-layer-services)
2. [File Transfer Protocol (FTP)](#2-file-transfer-protocol-ftp)
   - [FTP Architecture & Components](#ftp-architecture--components)
   - [FTP Connection Types: Control vs Data Connection](#ftp-connection-types-control-vs-data-connection)
   - [FTP Commands & Reply Codes](#ftp-commands--reply-codes)
3. [Electronic Mail System](#3-electronic-mail-system)
   - [Email System Architecture & Components (UA & MTA)](#email-system-architecture--components-ua--mta)
   - [Simple Mail Transfer Protocol (SMTP)](#simple-mail-transfer-protocol-smtp)
   - [Mail Message Format](#mail-message-format)
   - [Mail Access Protocols: POP3 vs IMAP4](#mail-access-protocols-pop3-vs-imap4)
4. [Domain Name System (DNS)](#4-domain-name-system-dns)
   - [Working & Name Resolution Mechanism](#working--name-resolution-mechanism)
   - [Services Provided by DNS](#services-provided-by-dns)
   - [DNS Resource Records](#dns-resource-records)
   - [DNS Message Format](#dns-message-format)
5. [Data Encryption Standard (DES)](#5-data-encryption-standard-des)
   - [DES Cryptographic Architecture](#des-cryptographic-architecture)
6. [Peer-to-Peer (P2P) Architecture & Applications](#6-peer-to-peer-p2p-architecture--applications)
   - [P2P Network Model & Working Principle](#p2p-network-model--working-principle)
   - [P2P File Distribution & Applications](#p2p-file-distribution--applications)
   - [Distributed Hash Table (DHT)](#distributed-hash-table-dht)
7. [Network Application Development & Socket Programming](#7-network-application-development--socket-programming)
   - [Socket Definition & Fundamentals](#socket-definition--fundamentals)
   - [Socket Programming with TCP](#socket-programming-with-tcp)
   - [Socket Programming with UDP](#socket-programming-with-udp)
8. [Important Definitions & Comparative Summary Tables](#8-important-definitions--comparative-summary-tables)
9. [Quick-Reference List of PDF Diagrams](#9-quick-reference-list-of-pdf-diagrams)
10. [Chapter Summary](#10-chapter-summary)

---

## 1. Introduction to Application Layer Services

The **Application Layer** occupies Top Layer (Layer 7) of the OSI reference model and TCP/IP protocol suite. It provides direct interface between end-user applications and underlying communication network services. Unlike lower layers that handle physical transmission, framing, routing, or end-to-end transport, application layer protocols define rules, message formats, and client-server or peer-to-peer interactions for network-aware software applications.

Key functions offered by Application Layer protocols include network file transfer, distributed directory lookup, electronic messaging, network management, remote login, decentralized peer resource sharing, and socket-based network application programming.

---

## 2. File Transfer Protocol (FTP)

**File Transfer Protocol (FTP)** is the standard application-layer mechanism provided by the TCP/IP protocol suite for copying and moving files from one host to another across an internetwork. It enables users to transfer web pages from creator machines to public web servers, download files from remote repositories to local storage, and manipulate remote file directory structures.

### FTP Architecture & Components

FTP utilizes a heterogeneous client-server architecture. The FTP model separates user commands from actual data transfer by employing distinct processes at both client and server ends.

The **FTP Client** consists of three core functional components:
1. **User Interface**: Interacts directly with the human user or client application, accepting commands (e.g., download, upload, directory listing) and displaying responses.
2. **Control Process (Client)**: Establishes and manages the control connection with the server control process, translating user inputs into standardized FTP commands.
3. **Data Transfer Process (Client)**: Manages the actual transmission and reception of data files or directory structures under instructions from the client control process.

The **FTP Server** consists of two core functional components:
1. **Control Process (Server)**: Listens for incoming connection requests on standard control ports, authenticates users, interprets received commands, and issues numerical reply codes.
2. **Data Transfer Process (Server)**: Listens for or initiates data connections to handle raw file transfers between the server file system and the client data transfer process.

**[📌 REFER TO PDF DIAGRAM: Mechanism of FTP – PDF PAGE NO. 2]**

### FTP Connection Types: Control vs Data Connection

FTP is distinct among network protocols because it uses **two separate TCP connections** between client and server during an active session:

1. **Control Connection**:
   - Used for transmitting control information such as user identification, authentication credentials, directory navigation commands, and server reply codes.
   - Remains active and open throughout the entire interactive FTP session.
   - Operates over standard **TCP Port 21**.
   - Transmits text commands one line at a time.

2. **Data Connection**:
   - Used exclusively for transferring actual data payload, including file content and directory listings.
   - Established dynamically when a file transfer command (e.g., `STOR`, `RETR`, `LIST`) is executed, and automatically closed as soon as the file transfer completes.
   - Operates over standard **TCP Port 20** (in active FTP mode).

| Feature / Criterion | Control Connection | Data Connection |
| :--- | :--- | :--- |
| **Primary Purpose** | Transferring commands, authentication credentials, and status replies | Transferring actual file data payloads and directory lists |
| **Standard Port Number** | TCP Port 21 | TCP Port 20 (Active Mode) |
| **Lifetime / Duration** | Persistent (remains open for the entire session duration) | Non-persistent (created on demand per file transfer, closed upon completion) |
| **Transmission Type** | Line-by-line ASCII control commands and numerical replies | Raw binary or ASCII file stream |

### FTP Commands & Reply Codes

Communication across the control connection consists of 4-letter uppercase ASCII commands sent by the client, followed by 3-digit numerical response codes sent by the server.

#### Primary FTP Commands
- **`USER`**: Sends the client user identification/username to the server.
- **`PASS`**: Sends the user authentication password to the server.
- **`CWD`**: (Change Working Directory) Allows the user to navigate to a different directory or dataset on the remote host.
- **`MKD`**: (Make Directory) Creates a new directory specified in the path name on the remote host.
- **`PWD`**: (Print Working Directory) Causes the name of the current working directory to be returned in the server reply.
- **`STOR`**: (Store) Initiates storage/upload of a file from client into current working directory of remote host.
- **`LIST`**: Requests a list of all files and subdirectories present in the current remote working directory.
- **`ABOR`**: (Abort) Instructs the server to abort the previous FTP service command and any associated active data transfer.
- **`QUIT`**: Terminates the user session; if no file transfer is in progress, server closes control connection.

#### Primary FTP Reply Codes
- **`200`**: Command okay (operation successful).
- **`530`**: Not logged in (authentication failure).
- **`331`**: User name okay, need password.
- **`225`**: Data connection open; transfer in progress.
- **`221`**: Service closing control connection (logged out).
- **`551`**: Requested action aborted; page type unknown.
- **`502`**: Command not implemented.
- **`503`**: Bad sequence of commands.
- **`504`**: Command not implemented for that parameter.

---

## 3. Electronic Mail System

The **Electronic Mail System (Email)** is a distributed network application enabling users to exchange formatted text messages, attachments, and data asynchronously over the Internet.

### Email System Architecture & Components (UA & MTA)

An email system comprises email addresses, client software, server storage, and specialized protocol agents:
- **Email Address**: Unique global identifier for a user, formatted as `name@domain.com`.
- **Email Client**: Software interface used by subscribers to compose, read, organize, and manage email messages.
- **Email Server**: Server system responsible for storing received messages in user mailboxes and forwarding outgoing messages to destination mail servers.

The functional system architecture is split into two primary components:

1. **User Agent (UA)**:
   - Also known as the **mail reader**, the UA is a client program that provides user interface for message preparation, editing, reading, replying, forwarding, and local mailbox manipulation.
2. **Message Transfer Agent (MTA)**:
   - Responsible for actual message transfer across intermediate systems from source host to destination mail server.
   - To send mail, a system must run a **Client MTA** (initiates connection) and destination system must run a **Server MTA** (receives mail). Delivery between MTAs is governed by **SMTP**.

**[📌 REFER TO PDF DIAGRAM: SMTP (Simple Mail Transfer Protocol) Architecture – PDF PAGE NO. 9]**

### Simple Mail Transfer Protocol (SMTP)

**Simple Mail Transfer Protocol (SMTP)** is an application-layer, TCP-based, connection-oriented protocol designed to send email reliably across heterogeneous networks.

- **Transport**: Operates over **TCP Port 25**. Once a TCP connection is established between client MTA and server MTA, multiple email messages can be delivered sequentially before closing the session.
- **Scope**: SMTP is primarily a push protocol used for sending mail from sender UA to sender MTA, and forwarding mail from sender MTA to recipient MTA.

#### Standard SMTP Commands
- **`HELO`**: Identifies sender host domain name to recipient MTA.
- **`MAIL`**: Specifies originator/sender address of the mail message (`MAIL FROM:`).
- **`RCPT`**: Specifies intended recipient address (`RCPT TO:`).
- **`DATA`**: Signals the start of actual mail text body input; terminated by a single line containing only a period (`.`).
- **`QUIT`**: Closes the active SMTP communication session.

### Mail Message Format

An electronic mail message consists of three structural parts:

1. **Envelope**: Encapsulates the mail message. Contains essential routing information required by MTAs, including destination IP/DNS address, priority level, and security parameters.
2. **Header**: Comprises key header fields defining sender and recipient metadata:
   - **`To:`**: Specifies DNS email address of primary recipient(s).
   - **`Cc:`**: (Carbon Copy) Specifies addresses of secondary recipients receiving identical copy.
   - **`Bcc:`**: (Blind Carbon Copy) Specifies secondary recipient addresses without revealing their identities to primary (`To`) or secondary (`Cc`) recipients.
   - **`From:`**: Specifies name and email address of the message author.
3. **Body**: Contains actual text payload and content intended for the recipient.

**[📌 REFER TO PDF DIAGRAM: Mail Message Format – PDF PAGE NO. 9]**

### Mail Access Protocols: POP3 vs IMAP4

While SMTP transfers mail to the recipient's mail server, the recipient's User Agent requires a **Mail Access Protocol** to retrieve messages from the remote mail server mailbox.

#### 1. Post Office Protocol (POP / POP3)
- Simple application-layer mail access protocol supporting client access to remote mailboxes over **TCP Port 110**.
- **Working Mechanism**: Downloads email messages from remote server to client computer's local disk drive and deletes/marks them on server.
- **Characteristics**: Supports offline message reading (minimizing active internet connection time); lacks server-side search capabilities; restricted to creating a single mailbox folder on server.

#### 2. Internet Message Access Protocol (IMAP / IMAP4)
- Advanced mail access protocol operating over **TCP Port 143**.
- **Working Mechanism**: Allows client UA to manipulate email messages directly on remote mail server without downloading whole messages locally.
- **Characteristics**: Maintains complete message repository on server; supports creating, renaming, and deleting hierarchical remote folders/mailboxes; enables server-side search and partial downloading.
- **Versions**: Evolved through five historical versions: Original IMAP, IMAP2, IMAP3, IMAP2bis, and **IMAP4**.

| Feature / Criterion | Post Office Protocol (POP3) | Internet Message Access Protocol (IMAP4) |
| :--- | :--- | :--- |
| **Primary Design Concept** | Download-and-delete protocol for offline reading | Interactive remote mailbox management protocol |
| **Server Synchronization** | Mail downloaded locally; server mailbox cleared or un-synced | Mail held and maintained centrally on remote server |
| **Folder Management** | Supports only one mailbox inbox on server | Supports creating, deleting, and manipulating remote folders |
| **Search Functionality** | No server-side searching (must download to search) | Powerful server-side searching and filtering capabilities |
| **Multi-Device Support** | Poor (downloaded messages locked to single client machine) | Excellent (state synchronized seamlessly across multiple devices) |
| **Standard TCP Port** | TCP Port 110 | TCP Port 143 |

---

## 4. Domain Name System (DNS)

Computers interact across IP networks using numerical binary/dotted-decimal addresses, whereas humans prefer mnemonic names. The **Domain Name System (DNS)** is an Internet directory service that resolves human-readable domain hostnames into numerical IP addresses.

### Working & Name Resolution Mechanism

When a user enters a domain name into a web browser:
1. The browser passes hostname (e.g., `www.example.com`) to local DNS client (resolver).
2. DNS resolver queries a DNS server to search local cache or hierarchical zone files for a matching record.
3. When resolved, DNS server returns target numerical IP address.
4. Browser opens direct TCP transport connection to destination web server IP address.

**[📌 REFER TO PDF DIAGRAM: Working of DNS – PDF PAGE NO. 16]**

### Services Provided by DNS

In addition to hostname-to-IP translation, DNS provides three crucial distributed infrastructure services:

1. **Host Aliasing**: A host with a complex canonical domain name can have one or more simpler alias names. For example, registering an alias domain pointing to a canonical domain routes users accessing the alias to the canonical domain.
2. **Mail Server Aliasing**: Allows organization mail servers to possess user-friendly email domain aliases that automatically forward incoming mail to target corporate mail infrastructure.
3. **Load Distribution**: Distributes client web requests across replicated server clusters. A single domain name configured in DNS maps to a pool of multiple IP addresses; DNS cycles through address lists in response to incoming queries.

### DNS Resource Records

DNS servers store mapping data in text-based database entries called **Resource Records (RRs)** on authoritative DNS servers. Each resource record contains four mandatory fields:
`(Name, Value, Type, TTL)`

- **`TTL` (Time To Live)**: Determines cached lifetime before record must be discarded from intermediate DNS caches.
- **`Type`**: Defines semantic meaning of `Name` and `Value` fields:
  - **`A Record`**: `Name` is hostname, `Value` is IPv4 address.
  - **`AAAA Record`**: `Name` is hostname, `Value` is 128-bit IPv6 address.
  - **`CNAME Record`**: `Name` is alias hostname, `Value` is canonical hostname.
  - **`NS Record`**: `Name` is domain, `Value` is hostname of authoritative name server.
  - **`MX Record`**: `Name` is domain, `Value` is mail server hostname associated with domain.

### DNS Message Format

DNS communication between clients and servers uses two message types:
1. **Query Message**: Sent by client resolver; contains Header (12 bytes) and Question Records.
2. **Response Message**: Sent by DNS server; contains Header (12 bytes), Question Records, Answer Records, Authoritative Records, and Additional Records.

**[📌 REFER TO PDF DIAGRAM: General DNS Message Format – PDF PAGE NO. 22]**

#### Structure of 12-Byte DNS Header
- **`Identification` (16 bits)**: Unique identifier assigned by client to match incoming server responses with outstanding queries.
- **`Flags` (16 bits)**: Specifies query/response status, operation code, authoritativeness, truncation, and recursion flags.
- **`Number of Question Records` (16 bits)**: Specifies number of queries present in Question section (present in query and response).
- **`Number of Answer Records` (16 bits)**: Specifies number of resource records in Answer section (present in response messages only).
- **`Number of Authoritative Records` (16 bits)**: Specifies count of resource records referencing authoritative DNS servers.
- **`Number of Additional Records` (16 bits)**: Holds additional helpful resource records provided by server to assist resolver.

---

## 5. Data Encryption Standard (DES)

Network application security relies on cryptographic encryption to protect message confidentiality. **Data Encryption Standard (DES)** is a classic symmetric-key block cipher algorithm.

### DES Cryptographic Architecture

DES processes input data in fixed **64-bit plaintext blocks** using a **56-bit cipher key** to yield a **64-bit ciphertext block**.

**[📌 REFER TO PDF DIAGRAM: General Structure of DES – PDF PAGE NO. 20]**

#### Step-by-Step Operation of DES:
1. **64-Bit Plaintext Input**: Input message is partitioned into 64-bit blocks.
2. **Initial Permutation (IP)**: Reorders the 64 bits of input block according to a fixed permutation table.
3. **16 Feistel Rounds**:
   - The permuted 64-bit block is split into left and right 32-bit halves.
   - The algorithm executes **16 identical rounds** of substitution and permutation operations.
   - In each round $i$ ($1 \le i \le 16$), a distinct **48-bit round key ($K_i$)** is derived from master **56-bit cipher key** through key-schedule generation algorithms.
4. **Final Permutation ($IP^{-1}$)**: After Round 16, halves are swapped and passed through inverse initial permutation ($IP^{-1}$).
5. **64-Bit Ciphertext Output**: Produces encrypted 64-bit output block.

---

## 6. Peer-to-Peer (P2P) Architecture & Applications

### P2P Network Model & Working Principle

Unlike conventional Client-Server models where centralized servers respond to passive clients, **Peer-to-Peer (P2P)** architecture connects host computers directly in a decentralized workgroup.

- **Peer Equality**: Every device (peer) in a P2P network possesses identical functional capabilities and responsibilities, acting simultaneously as both **Client** and **Server**.
- **Decentralization**: Nodes store data on local storage drives and share internet access or files without relying on a central database server.

**[📌 REFER TO PDF DIAGRAM: P2P Architecture – PDF PAGE NO. 24]**

#### Working Procedure of P2P Networks:
1. **Connection**: Peers join network by establishing direct IP connections with existing neighbor peers.
2. **Resource Sharing**: Peers designate specific local directories and resources for public access.
3. **Discovery**: Peers search and locate desired content by sending distributed search queries across connected peers.
4. **Direct Transfer**: Content files are downloaded directly from holding peers (uploaders) to requesting peers (downloaders).

### P2P File Distribution & Applications

P2P file sharing software allows user nodes to download content files while simultaneously uploading downloaded chunks to other searching peers.

#### Major Applications of P2P Networks:
1. **File Sharing**: High-speed distribution of large software packages, documents, books, music, and media files.
2. **Online Gaming**: Multi-player real-time state synchronization across gaming nodes.
3. **Social Networking**: Distributed community messaging and status updates.
4. **Voice & Video Calls**: Peer-to-peer VoIP communications.
5. **Data Backup & Storage**: Distributed redundant data archival across idle node hard drives.

### Distributed Hash Table (DHT)

A **Distributed Hash Table (DHT)** is a decentralized storage mechanism providing fast lookup and key-value storage across P2P networks.

- **Working Mechanism**:
  - Raw data payload is passed through a **Hash Function** to generate a unique fixed-length **Key**.
  - Key-Value pairs are distributed across network nodes; each participating node maintains responsibility for a specific range of keys.
  - Any node can retrieve value mapped to key efficiently without central indexing.

**[📌 REFER TO PDF DIAGRAM: Distributed Hash Table – PDF PAGE NO. 27]**

#### Essential Properties of DHT:
- **Decentralized & Automated**: Self-organizing node distribution without central coordination.
- **Fault Tolerant**: Handles node join/leave churn without data loss.
- **Scalable**: Maintains performance across millions of active peer nodes.

---

## 7. Network Application Development & Socket Programming

### Socket Definition & Fundamentals

A **Socket** serves as an application programming interface (API) endpoint enabling process-to-process communication across a network.

$$\text{Socket} = \text{IP Address} + \text{Software Port Number}$$

Sockets form the software interface between application process layer and underlying operating system transport layer protocols (TCP or UDP).

### Socket Programming with TCP

TCP socket programming provides reliable, connection-oriented, byte-stream communication with built-in flow control and packet ordering.

#### TCP Server API Calls:
1. **`socket()`**: Instantiates a TCP stream socket descriptor.
2. **`bind()`**: Associates socket descriptor with server local IP address and port number.
3. **`listen()`**: Configures socket into passive listening mode to queue connection requests.
4. **`accept()`**: Blocks until incoming client connection request arrives; returns new connected socket descriptor.
5. **`send()` / `recv()`**: Transmits and receives data over established connection.

#### TCP Client API Calls:
1. **`socket()`**: Instantiates client TCP socket descriptor.
2. **`connect()`**: Initiates 3-way handshake to connect client socket to remote server IP and port.
3. **`send()` / `recv()`**: Data exchange with server.
4. **`close()`**: Terminates socket session.

**[📌 REFER TO PDF DIAGRAM: Socket Programming with TCP Flowchart – PDF PAGE NO. 30]**

### Socket Programming with UDP

UDP socket programming provides connectionless, unreliable datagram transmission without establishing connection or maintaining order state.

#### UDP Server API Procedure:
1. **`socket()`**: Creates UDP datagram socket descriptor.
2. **`bind()`**: Binds socket to server IP and port number.
3. **`recvfrom()`**: Blocks waiting for incoming datagram packet from any client.
4. **`sendto()`**: Sends response datagram packet back to client address.
5. **`close()`**: Releases socket descriptor.

#### UDP Client API Procedure:
1. **`socket()`**: Creates UDP datagram socket descriptor.
2. **`bind()`**: (Optional) Binds client socket.
3. **`sendto()`**: Transmits datagram packet directly to target server IP and port address.
4. **`recvfrom()`**: Waits for server response datagram.
5. **`close()`**: Closes socket descriptor.

**[📌 REFER TO PDF DIAGRAM: Socket Programming with UDP Flowchart – PDF PAGE NO. 31]**

---

## 8. Important Definitions & Comparative Summary Tables

### Key Technical Definitions

- **File Transfer Protocol (FTP)**: Standard TCP/IP application-layer protocol for moving files between local and remote file systems using separate control and data connections.
- **User Agent (UA)**: Application program (mail reader) enabling users to compose, send, receive, and manage email messages.
- **Message Transfer Agent (MTA)**: System software responsible for transferring emails across mail servers using SMTP.
- **Simple Mail Transfer Protocol (SMTP)**: Connection-oriented application protocol over TCP Port 25 used to push emails between servers.
- **Domain Name System (DNS)**: Hierarchical distributed directory service mapping human-readable domain hostnames to numerical IP addresses.
- **Data Encryption Standard (DES)**: Symmetric-key block cipher encrypting 64-bit plaintext blocks into 64-bit ciphertext blocks using a 56-bit key across 16 Feistel rounds.
- **Peer-to-Peer (P2P)**: Decentralized network model where interconnected nodes share equal responsibilities as both client and server.
- **Distributed Hash Table (DHT)**: Decentralized lookup storage system mapping keys to values across P2P network nodes.
- **Socket**: Communication end-point defined by combination of IP Address and Port Number (`IP:Port`).

### Comparative Summary: TCP vs UDP Socket Programming

| Feature / Criterion | TCP Socket Programming | UDP Socket Programming |
| :--- | :--- | :--- |
| **Protocol Mode** | Connection-oriented (requires `connect` / `accept` handshake) | Connectionless (no handshake required) |
| **Data Transmission API** | `send()` and `recv()` over established stream | `sendto()` and `recvfrom()` with explicit destination parameters |
| **Reliability & Ordering** | High (guarantees ordered, loss-free byte delivery) | Unreliable (datagrams may arrive out-of-order or be lost) |
| **Server State** | Maintains socket connection state for each connected client | Stateless (handles individual independent datagram packets) |
| **Overhead & Speed** | Higher header overhead and connection delay | Minimal header overhead and fast datagram processing |

---

## 9. Quick-Reference List of PDF Diagrams

For university examination diagram preparation, refer directly to the exact page numbers in `05_FIFTH-CHAPTER.pdf`:

1. **[📌 REFER TO PDF DIAGRAM: Mechanism of FTP – PDF PAGE NO. 2]**
2. **[📌 REFER TO PDF DIAGRAM: SMTP (Simple Mail Transfer Protocol) Architecture – PDF PAGE NO. 9]**
3. **[📌 REFER TO PDF DIAGRAM: Mail Message Format – PDF PAGE NO. 9]**
4. **[📌 REFER TO PDF DIAGRAM: Working of DNS – PDF PAGE NO. 16]**
5. **[📌 REFER TO PDF DIAGRAM: General Structure of DES – PDF PAGE NO. 20]**
6. **[📌 REFER TO PDF DIAGRAM: General DNS Message Format – PDF PAGE NO. 22]**
7. **[📌 REFER TO PDF DIAGRAM: P2P Architecture – PDF PAGE NO. 24]**
8. **[📌 REFER TO PDF DIAGRAM: Distributed Hash Table – PDF PAGE NO. 27]**
9. **[📌 REFER TO PDF DIAGRAM: Socket Programming with TCP Flowchart – PDF PAGE NO. 30]**
10. **[📌 REFER TO PDF DIAGRAM: Socket Programming with UDP Flowchart – PDF PAGE NO. 31]**

---

## 10. Chapter Summary

Unit V covers key protocols, architectural paradigms, security primitives, and programming APIs at the **Application Layer**:

- **FTP**: Employs client components (User Interface, Control Process, Data Transfer Process) and server components (Control Process, Data Transfer Process). Uses persistent **Control Connection (TCP Port 21)** for commands and temporary **Data Connection (TCP Port 20)** for file payload.
- **Email System**: Architecture splits into User Agent (UA) for message creation and Message Transfer Agent (MTA) for SMTP transmission over **TCP Port 25**. Mail retrieval relies on **POP3 (TCP Port 110)** for download-and-delete offline access, or **IMAP4 (TCP Port 143)** for interactive remote mailbox folder management.
- **DNS**: Resolves hostnames to IP addresses using resource records (`A`, `AAAA`, `CNAME`, `NS`, `MX`). Employs a 12-byte header containing Identification, Flags, and Question/Answer/Authoritative/Additional record counts.
- **DES**: Block cipher converting 64-bit plaintext into 64-bit ciphertext using a 56-bit cipher key via Initial Permutation, 16 Feistel rounds with 48-bit subkeys, and Final Permutation.
- **P2P Architecture**: Decentralized workgroup where every node operates as both client and server. Uses **Distributed Hash Tables (DHT)** with hash key mappings for fault-tolerant, scalable lookup.
- **Socket Programming**: Sockets pair IP Address and Port Number. TCP socket APIs (`socket`, `bind`, `listen`, `accept`, `connect`, `send`/`recv`) guarantee reliable connection stream delivery; UDP socket APIs (`socket`, `bind`, `sendto`, `recvfrom`) manage low-overhead, connectionless datagram transmission.
