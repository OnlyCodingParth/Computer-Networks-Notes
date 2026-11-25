# CHAPTER 5 (UNIT V) – APPLICATION LAYER

---

## Module 1: File Transfer Protocol (FTP)

### Topic A: Definition, Components & Mechanism of FTP

- **File Transfer:** Transferring a file from one computer to another allows users to communicate and exchange information between two physical devices.
- **FTP Definition:** **File Transfer Protocol (FTP)** is the standard mechanism provided by **TCP/IP** for copying a file from one host to another. It is an **application layer protocol** that moves files between local and remote file systems.
- **Primary Uses:** It is mainly used for **transferring web page files** from their creator to a server, as well as **downloading files** to a computer from other servers.

#### FTP Architecture & Components

- **FTP Client (3 Components):** **User Interface**, **Control Process**, and **Data Transfer Process**.
- **FTP Server (2 Components):** **Control Process** and **Data Transfer Process**.

**Standard Exam Diagram (Mechanism of FTP):**

```text
       FTP CLIENT                                             FTP SERVER
+-----------------------+                               +-----------------------+
|    User Interface     |                               |                       |
+-----------------------+                               |                       |
|    Control Process    | <==== Control (Port 21) ====> |    Control Process    |
+-----------------------+         (TCP/IP)              +-----------------------+
| Data Transfer Process | <===== Data (Port 20) ======> | Data Transfer Process |
+-----------------------+         (TCP/IP)              +-----------------------+
```

#### Two Types of Connections in FTP

1. **Control Connection (TCP Port 21):**
   - Used to transfer **one line of command or response at a time**.
   - Carries control information such as **user identification, password, directory-change commands, and commands to retrieve or store files**.
   - Remains **open throughout the entire interactive session**.
2. **Data Connection (TCP Port 20):**
   - Used exclusively for the **actual file transfer process**.
   - Opens dynamically when a file transfer is needed and **closes automatically as soon as the file transfer completes**.

#### Common FTP Reply Codes

- **`200`** : Command okay
- **`331`** : User name okay, need a password
- **`225`** : Data connection open and transfer in progress
- **`221`** : Service closing control connection
- **`530`** : Not logged in

---

### Topic B: FTP Commands & Replies

#### 1. `USER` (User Identification) — [1 Mark]

- **Function:** Sends the client's user identification (username) to the FTP server to begin authentication.
- **Session Behaviour:** It is transmitted as a line-by-line ASCII command over the persistent control connection. Upon accepting the username, the server typically replies with code **`331`** (*"User name okay, need a password"*).

#### 2. `PASS` (Password) — [1 Mark]

- **Function:** Sends the user's authentication password to the FTP server.
- **Session Behaviour:** Used immediately after the `USER` command over the control connection. If credentials are valid, the server proceeds (reply **`200`** – *Command okay*); if authentication fails, the server returns **`530`** (*"Not logged in"*).

> `[VERIFY]` Teacher's note gives `200` as the success reply to `PASS`. Preserved as written (see Verification Notes at the end).

#### 3. `STOR` (Store File) — [1 Mark]

- **Function:** Initiates the storage (upload) of a file from the local client into the current working directory of the remote host.
- **Session Behaviour:** While the `STOR` command itself is sent over the control connection, transferring the actual file payload triggers FTP to open a temporary **Data Connection (TCP Port 20)**, which closes automatically once the file transfer completes.

#### 4. `QUIT` (Terminate Session) — [1 Mark]

- **Function:** Terminates the active `USER` session.
- **Session Behaviour:** If no file transfer is currently in progress, the server closes the control connection and sends reply code **`221`** (*"Service closing control connection"*).

#### 5. `CWD` (Change Working Directory)

- **Function:** Allows the user to navigate to and work with a different directory or dataset on the remote host for file storage or accounting information.
- **Session Behaviour:** Sent as a 4-letter uppercase ASCII command over the persistent **Control Connection (TCP Port 21)**, which remains open throughout the session.

#### 6. `MKD` (Make Directory)

- **Function:** Creates a new directory specified in the path name on the remote host.
- **Session Behaviour:** Transmitted line-by-line over the **Control Connection (TCP Port 21)**, after which the server responds with a 3-digit numerical reply code (such as `200` for command okay, or an error code like `502`/`503`/`504` if not implemented or invalid).

#### 7. `PWD` (Print Working Directory)

- **Function:** Requests that the name of the current working directory be returned in the server's reply.
- **Session Behaviour:** Executed over the **Control Connection (TCP Port 21)** without opening a data connection, since the directory path is returned directly in the server's control reply.

#### 8. `LIST` (List Files)

- **Function:** Requests a list of all files and subdirectories present in the current remote working directory.
- **Session Behaviour:** The `LIST` command itself is sent over the **Control Connection (TCP Port 21)**, while the actual directory listing is transferred over a dynamically created **Data Connection (TCP Port 20)** that closes automatically once the transfer finishes.

#### 9. `ABOR` (Abort)

- **Function:** Instructs the server to abort the previous FTP service command along with any associated active data transfer.
- **Session Behaviour:** Sent over the **Control Connection (TCP Port 21)** to interrupt an ongoing operation on the data connection.

#### Quick Exam Tip (Grouping the 9 Commands)

If you ever need to pick any 4 commands in the exam, you can group them mentally by role:

- **Login & Exit:** `USER`, `PASS`, `QUIT`
- **Directory Navigation:** `CWD`, `MKD`, `PWD`, `LIST`
- **File Transfer & Control:** `STOR`, `ABOR`

---

## Module 2: Electronic Mail & SMTP

> `[VERIFY]` The "Module 2" heading was missing in the pasted notes. It is inferred from the sequence (Module 1 → Module 3). Please confirm the module number and title against your teacher's notes.

### Topic A: Electronic Mail in the Internet & Its Components

#### 1. Definition & Basics of Email

- **Electronic Mail (Email)** is a widely used internet service for exchanging formatted text messages, attachments, and data asynchronously between users anywhere in the world.
- Every email setup relies on three basic elements:
  - **Email Address:** A unique identifier for each user, typically formatted as `name@domain.com`.
  - **Email Client:** Software used by subscribers to send, receive, and manage emails.
  - **Email Server:** A computer system responsible for storing incoming emails in mailboxes and forwarding outgoing emails to intended recipients.

#### 2. Core Components of an Email System

- **User Agent (UA):** Also called a **mail reader**, the UA is a client program responsible for message preparation. It accepts user commands for composing, receiving, replying to, forwarding, and manipulating local mailboxes.
- **Message Transfer Agent (MTA):** Responsible for transferring mail from one system to another across the network. Sending mail requires both a **Client MTA** (to initiate transfer) and a **Server MTA** (to receive mail), with delivery between MTAs governed by **SMTP**.

---

### Topic B: Simple Mail Transfer Protocol (SMTP) & Commands

#### 1. Overview & Working of SMTP

- **Simple Mail Transfer Protocol (SMTP)** is a connection-oriented, text-based application-layer protocol used to send email reliably and efficiently over the Internet.
- It acts as a **push protocol** that sends mail from the sender's UA to the sender's MTA, and forwards it from the sender's MTA to the recipient's MTA.
- SMTP defines communication rules between servers so they can identify themselves and exchange messages between users on the same or different computers.
- It operates over **TCP Port 25**; once a TCP connection is established, multiple emails can be sent sequentially before closing the session.

#### 2. Standard Exam Diagram (SMTP Architecture)

```text
    +--------+                                        +--------+
    | User A |                                        | User B |
    +---+----+                                        +---+----+
        |                                                 |
        v                                                 v
+---------------+                                 +---------------+
|  User Agent   |                                 |  User Agent   |
|     (UA)      |                                 |     (UA)      |
+-------+-------+                                 +-------+-------+
        v                                                 |
+---------------+         +-------------+         +---------------+
|    Message    |         |             |         |    Message    |
|   Transfer    | <=====> |  Internet   | <=====> |   Transfer    |
|  Agent (MTA)  |  SMTP   |(TCP Pt 25)  |  SMTP   |  Agent (MTA)  |
+---------------+         +-------------+         +---------------+
```

> `[DIAGRAM NEEDS VERIFICATION]` The original diagram was badly distorted. The boxes and labels above were rebuilt from what survived (User A / User B → UA → MTA ⇄ Internet (TCP Pt 25) ⇄ MTA). The right-hand side arrows (User B → UA, UA → MTA) are drawn exactly as in the pasted notes (downward arrow at the top, plain line below). Please check the direction of the right-hand arrows against your teacher's diagram.

#### 3. Standard SMTP Commands

1. **`HELO`** : Identifies the sender host domain name to the receiving MTA.
2. **`MAIL`** : Specifies the originator/sender email address (`MAIL FROM:`).
3. **`RCPT`** : Specifies the intended recipient's email address (`RCPT TO:`).
4. **`DATA`** : Signals the beginning of the actual mail body text (ended by a single line containing a period `.`).
5. **`QUIT`** : Indicates that the conversation is over and closes the active SMTP session.

---

## Module 3: Mail Message Format & Access Protocols (POP3 vs. IMAP4)

### Topic A: Mail Message Format (Format of Email)

*(Matches: "Draw and Explain mail Message Format" / "Outline the Mail Message Format" — **4 Marks**)*

An electronic mail message consists of **three structural parts**: **Envelope**, **Header**, and **Body**.

**Standard Exam Diagram (Mail Message Format):**

```text
+-----------------------------------------+
|                Envelope                 |  <-- Routing info (Destination, Priority, Security)
|   +---------------------------------+   |
|   |             Header              |   |
|   +---------------------------------+   |
|   |                                 |   |
|   |              Body               |   |
|   |                                 |   |
|   +---------------------------------+   |
+-----------------------------------------+
```

> `[DIAGRAM NEEDS VERIFICATION]` In the pasted notes only the outer "Envelope" label and the routing-information note survived. The inner box labels were lost. "Header" and "Body" are restored from the text below ("The envelope encapsulates the mail message"). Please confirm against your teacher's diagram.

#### 1. Envelope (1 Mark)

- The envelope encapsulates the mail message.
- It contains all essential routing information required by Message Transfer Agents (MTAs) to send the email, such as the **destination address, priority, and security level**.

#### 2. Header (2 Marks)

- The header consists of a series of lines defining sender and recipient metadata. The main header fields are:
  - **`To:`** — Specifies the DNS email address of the **primary recipient**.
  - **`Cc:`** **(Carbon Copy)** — Specifies the addresses of **secondary receivers** receiving an identical copy.
  - **`Bcc:`** **(Blind Carbon Copy)** — Similar to `Cc:`, except it allows the sender to send a copy to a third party **without the primary and secondary recipients knowing** about it.
  - **`From:`** — Specifies the **name (and email address) of the person who wrote the message**.

#### 3. Body (1 Mark)

- The body contains the text that forms the **actual content/message** intended for the recipient.

---

### Topic B: Mail Access Protocols (SMTP, IMAP4 & POP3)

*(Matches: "Explain Message Access Protocols" — **4 Marks**)*

Email protocols are a collection of protocols used to send and receive emails properly. While SMTP pushes mail to the recipient's mail server, the recipient's User Agent (UA) uses a **Mail Access Protocol** to retrieve messages from the remote mailbox.

**Hierarchy Diagram (Types of Email Protocols):**

```text
              +-------------------------+
              | Types of Email Protocol |
              +------------+------------+
                           |
         +-----------------+-----------------+
         |                 |                 |
         v                 v                 v
     [ SMTP ]          [ IMAP ]           [ POP ]
```

#### 1. SMTP (Simple Mail Transfer Protocol)

- An application-layer, connection-oriented, text-based protocol operating over **TCP Port 25**.
- Used for sending email efficiently and reliably over the Internet, providing notifications regarding incoming mail and using commands to specify sender/receiver addresses and message content.

#### 2. IMAP / IMAP4 (Internet Message Access Protocol)

- Operates over **TCP Port 143**.
- Allows the client program to **manipulate email messages directly on the remote server** without downloading them onto the local computer.
- Emails are held and maintained by the remote server, enabling users to download or delete messages without reading them first, and to **create, manipulate, and delete remote message folders (mailboxes)**.
- **Five Versions of IMAP:** Original IMAP, IMAP 2, IMAP 3, IMAP 2bis, and **IMAP 4**.

#### 3. POP / POP3 (Post Office Protocol)

- Operates over **TCP Port 110**.
- Generally used to support a **single client** and supports **offline access** to messages (downloading them to local storage and deleting/marking them on the server), which requires less internet usage time.
- **Limitations:** POP does **not** allow a server-side search facility (messages must be downloaded first to be accessed) and allows only **one mailbox** to be created on the server.

---

## Module 4: Domain Name System (DNS)

### A. Definition & Purpose (1 Mark)

- The **Domain Name System (DNS)** is an **Internet directory service** that provides a mapping between the human-readable name of a host on the network and its numerical **IP address**.
- In networking, computers do not identify each other by names like humans do; instead, they communicate using numerical IP addresses.
- DNS acts like a **phonebook**: you look up the domain name first, and it gives you the corresponding numerical IP address.

### B. Standard Exam Diagram: Working of DNS (1 Mark)

```text
+---------------+      1. www.abc.com       +---------------+
|               | ------------------------> |               |
|  Web Browser  |                           |  DNS Server   |
|               | <------------------------ |               |
+---------------+                           +---------------+
        |  ^
        v  |  3. Connect using IP (15.20.30.51)
+---------------+     & retrieve web page
|  Web Server   |
+---------------+
```

*(Reference: fig - Working of DNS, Page 16 of `05_FIFTH-CHAPTER.pdf`)*

> `[DIAGRAM NEEDS VERIFICATION]` The returning arrow (DNS Server → Web Browser) has no label in the pasted notes, although labels "1." and "3." are present. A label "2." appears to be missing. It is left blank here rather than guessed. Please check your teacher's diagram.

### C. Step-by-Step Working Mechanism (2 Marks)

When a user types a web address (such as `google.com` or `www.abc.com`) into a web browser:

1. **Query Submission:** The browser passes the domain name to the DNS server (via the local DNS resolver).
2. **Cache / Zone Lookup:** The DNS server searches through its cache or hierarchical zone files to find a matching IP address for that domain name.
3. **IP Resolution:** Once found, the DNS server resolves the domain name and returns the numerical IP address to the browser.
4. **Web Server Connection:** Using the resolved IP address, the user's computer communicates directly with the target web server to retrieve the requested web page.

### D. Services Provided by DNS

- **Host Aliasing:** When two or more domain names refer to the same IP address, it is called domain/host aliasing. An organization can register an additional alias domain name (e.g., `test.net`) that points to the IP/location of its primary canonical domain (e.g., `test.com`), so users visiting the alias land on the main site.
- **Mail Server Aliasing:** An email alias created under a mail server automatically forwards all incoming mail to another email address owned and controlled by the user/organization.
- **Load Distribution:** Distributes client requests across a group of replicated server machines by configuring a single domain in DNS to correspond to multiple IP addresses for an online service or mail system.

---

## Module 5: Peer-to-Peer (P2P) Architecture & Distributed Hash Tables (DHT)

### Topic A: Peer-to-Peer (P2P) Network & Applications

#### A. Definition & Core Principle (1 Mark)

- In the **Peer-to-Peer (P2P)** network architecture, computers connect directly with one another in a decentralized workgroup to share files as well as access to the internet and printers.
- Every computer in a P2P network has the **same set of responsibilities and capabilities**, meaning each device serves simultaneously as **both a client and a server**.
- This architecture is particularly useful in **residential areas, small offices, and small companies**, where each computer acts as an independent workstation storing data on its own hard drives rather than relying on a central server.

#### B. Standard Exam Diagram: P2P Architecture (1 Mark)

```text
  +-----+                 +-----+
  |  A  |-----------------|  B  |
  +-----+                 +-----+
     |    \             /    |
     |      \         /      |
     |        \     /        |
     |          \ /          |
     |           X           |
     |          / \          |
     |        /     \        |
     |      /         \      |
     |    /             \    |
  +-----+                 +-----+
  |  C  |-----------------|  D  |
  +-----+                 +-----+

       fig - P2P Architecture
```

*(Each node A, B, C, D acts as both client and server, sharing resources directly)*

#### C. How a P2P Network Works (1 Mark)

A P2P network operates through four sequential steps:

1. **Connection:** Peers connect to the network.
2. **Resource Sharing:** Peers share their local resources (such as designated drives or folders).
3. **Discovery:** Peers search for and locate desired resources across connected peers.
4. **Direct Transfer:** Resources are shared directly between peers without passing through a central server.

#### D. Applications of P2P Networks (1 Mark)

1. **File Sharing:** Distributing and downloading files across users.
2. **Online Gaming:** Synchronizing multiplayer gaming across nodes.
3. **Social Networking:** Distributed community messaging and updates.
4. **Voice and Video Calls:** Direct peer-to-peer VoIP communication.
5. **Data Backup and Storage:** Distributed data archival across peers.

---

### Topic B: Distributed Hash Table (DHT)

#### 1. Definition & Working Mechanism

- A **Distributed Hash Table (DHT)** is a **decentralized storage system** that provides lookup and storage schemes similar to a hash table by storing **key-value pairs**.
- Raw **Data** is passed through a **Hash Function** to produce a unique fixed-length **Key**, which is mapped across the distributed network.
- Each node in a DHT is responsible for a specific key along with its mapped values, allowing any node to efficiently retrieve the value associated with a given key. The values mapped against keys can be any arbitrary form of data.

#### 2. Standard Exam Diagram: Distributed Hash Table

```text
  +--------+     +---------------+     +-----------+
  |  Data  | --> | Hash Function | --> |  Key 1    | ----\
  +--------+     +---------------+     +-----------+      \
                                                           v
  +--------+     +---------------+     +-----------+    ( Distributed )
  |  Data  | --> | Hash Function | --> |  Key 2    | -> (   Network   )
  +--------+     +---------------+     +-----------+    (             )
                                                           ^
  +--------+     +---------------+     +-----------+      /
  |  Data  | --> | Hash Function | --> |  Key 3    | ----/
  +--------+     +---------------+     +-----------+

             fig - Distributed Hash Table
```

#### 3. Essential Properties of DHT

- **Decentralised & Automated:** Self-organizing without central coordination.
- **Fault Tolerant:** Continues operating reliably even when nodes join or leave.
- **Scalable:** Maintains lookup performance across large numbers of participating nodes.

---

## Module 6: Network Application Development & Socket Programming

### Socket Programming with TCP

#### A. Overview & TCP Functionalities (1 Mark)

- **TCP (Transmission Control Protocol)** is a **reliable, connection-oriented** transport-layer protocol that establishes a connection before transmitting data.
- Creating a client-server connection using TCP provides three key functionalities:
  1. **Flow control**
  2. **Reliability**
  3. **Ordered delivery** (rearranges data packets in the specified order)

#### B. Standard Exam Diagram: Socket Programming with TCP (1 Mark)

```text
         SERVER                                  CLIENT
  +------------------+                    +------------------+
  |    socket()      |                    |    socket()      |
  +--------+---------+                    +--------+---------+
           |                                       |
           v                                       |
  +------------------+                             |
  |  setsockopt()    |                             |
  +--------+---------+                             |
           |                                       |
           v                                       |
  +------------------+                             |
  |     Bind()       |                             |
  +--------+---------+                             |
           |                                       |
           v                                       v
  +------------------+                    +------------------+
  |    Listen()      | <----------------- |    connect()     |
  +--------+---------+                    +--------+---------+
           |                                       |
           v                                       |
  +------------------+                             |
  |    Accept()      |                             |
  +--------+---------+                             |
           |                                       |
           v                                       v
  +------------------+                    +------------------+
  |   send / recv    | <================> |   send / recv    |
  +------------------+                    +------------------+
```

*(Reference: fig - Socket Programming with TCP, Page 30 of `05_FIFTH-CHAPTER.pdf`)*

#### C. TCP Server & Client Operations (2 Marks)

**TCP Server Side:**

1. **`Create()` / `socket()`** : Creates a TCP socket.
2. **`Bind()`** : Binds the socket to the server's local IP address and port number.
3. **`Listen()`** : Puts the server socket in passive mode, where it waits for clients to approach the server to make a connection.
4. **`accept()`** : Establishes the connection between client and server so they are ready to transfer data.
5. **`send()` / `recv()`** : Transmits and receives data over the established connection.

**TCP Client Side:**

1. **`create()` / `socket()`** : Creates a TCP socket.
2. **`connect()`** : Connects the newly created client socket to the server.
3. **`send()` / `recv()`** : Exchanges data with the server.
4. **`close()`** : Terminates the socket session.

---

### Socket Programming with UDP

#### A. Overview of UDP (1 Mark)

- **UDP (User Datagram Protocol)** is a **connectionless and unreliable** transport-layer protocol.
- Unlike TCP, UDP **does not establish a connection** between two communicating devices before transmitting data.

#### B. Standard Exam Diagram: Socket Programming with UDP (1 Mark)

```text
         SERVER                                  CLIENT
  +------------------+                    +------------------+
  |    socket()      |                    |    socket()      |
  +--------+---------+                    +--------+---------+
           |                                       |
           v                                       v
  +------------------+                    +------------------+
  |     Bind()       |                    |     Bind()       | <-- Optional
  +--------+---------+                    +--------+---------+
           |                                       |
           v                                       v
  +------------------+                    +------------------+
  |   recvfrom()     | <----------------- |    sendto()      |
  +--------+---------+                    +--------+---------+
           |                                       |
           v                                       v
  +------------------+                    +------------------+
  |    sendto()      | -----------------> |   Recvfrom()     |
  +--------+---------+                    +--------+---------+
           |                                       |
           v                                       v
  +------------------+                    +------------------+
  |     close()      |                    |     close()      |
  +------------------+                    +------------------+
```

*(Reference: fig - Socket Programming with UDP, Page 31 of `05_FIFTH-CHAPTER.pdf`)*

#### C. UDP Server & Client Operations (2 Marks)

**UDP Server Side:**

1. Create a UDP socket (`socket()`).
2. Bind the socket to the server address (`Bind()`).
3. Wait until the datagram packet arrives from the client (`recvfrom()`).
4. Process the datagram packet and send a reply to the client (`sendto()`).
5. Go back to step 3 (or call `close()` to release the socket descriptor).

**UDP Client Side:**

1. Create a UDP socket (`socket()`). *(Binding via `Bind()` is optional on the client side)*
2. Send a message to the server (`sendto()`).
3. Wait until a response from the server is received (`Recvfrom()`).
4. Process the reply and go back to step 2, if necessary.
5. Close the socket descriptor and exit (`close()`).

---

## Editor's Verification Notes

These items were **not changed** in the content. They are flagged only so you can compare them with your teacher's original notes.

1. **Module 2 heading missing.** The pasted notes had no "Module 2" heading before the Email topics. "Module 2: Electronic Mail & SMTP" is inferred from the sequence and the topics. Please confirm.
2. **`PASS` success reply code.** The notes give `200` (*Command okay*) as the reply when credentials are valid. Please confirm this against your teacher's answer. The text is preserved exactly as written.
3. **SMTP diagram (right-hand arrows).** Direction of the arrows on the User B / UA / MTA side is as pasted. See the `[DIAGRAM NEEDS VERIFICATION]` note under the diagram.
4. **Mail Message Format diagram.** Inner box labels (Header, Body) were lost in copying and were restored from the text. See the note under the diagram.
5. **DNS diagram.** The label on the returning arrow (presumably "2.") is missing in the pasted notes. See the note under the diagram.
6. **TCP diagram vs. text.** The diagram shows `setsockopt()` on the server side, which is not listed in the "TCP Server Side" steps. The text lists `close()` on the client side, which is not shown in the diagram. Both are preserved as in the notes.
7. **Numbering/labels normalised (formatting only).** The "Topic A" label was added to the P2P section (the original had "1. Peer-to-Peer…" followed by "Topic B"). The Email topics were labelled "Topic A/B" under Module 2, and Module 1 was likewise labelled "Topic A/B".
8. **Copy-paste artefacts removed.** These were the bracketed reference numbers such as `[3][4]`, stray `\*\*` markers, and HTML entities (`&lt;`, `&gt;`, `&amp;`). No wording was changed.
