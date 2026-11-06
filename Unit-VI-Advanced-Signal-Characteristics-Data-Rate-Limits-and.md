# Unit VI – Advanced Network Security, Cryptography and Perimeter Protection

## Table of Contents
1. [Clarification Note on Syllabus Mapping](#1-clarification-note-on-syllabus-mapping)
2. [Introduction to Network Security & CIA Triad](#2-introduction-to-network-security--cia-triad)
3. [Symmetric Key Cryptography](#3-symmetric-key-cryptography)
4. [Data Encryption Standard (DES)](#4-data-encryption-standard-des)
5. [Advanced Encryption Standard (AES)](#5-advanced-encryption-standard-aes)
6. [Asymmetric Key Cryptography (Public Key Cryptography)](#6-asymmetric-key-cryptography-public-key-cryptography)
7. [RSA Algorithm & Step-by-Step Solved Example](#7-rsa-algorithm--step-by-step-solved-example)
8. [User Authentication & Hash Functions](#8-user-authentication--hash-functions)
9. [Digital Signatures](#9-digital-signatures)
10. [Firewalls & Network Perimeter Security](#10-firewalls--network-perimeter-security)
11. [Important Definitions & Comparative Summary Tables](#11-important-definitions--comparative-summary-tables)
12. [Quick-Reference List of PDF Diagrams](#12-quick-reference-list-of-pdf-diagrams)
13. [Chapter Summary](#13-chapter-summary)

---

## 1. Clarification Note on Syllabus Mapping

This study guide is derived directly from `06_SIXTH-CHAPTER.pdf`. While some generic syllabus outlines group physical transmission limits and data link protocols into later units, the actual curriculum text and examination material in `06_SIXTH-CHAPTER.pdf` covers **Unit VI: Network Security, Cryptography, Hash Functions, Digital Signatures, and Firewalls**. The physical layer transmission parameters, signal impairments, Nyquist/Shannon capacity formulas, framing methods (byte/bit stuffing), and link control ARQ protocols (Stop-and-Wait, Go-Back-N, Selective Repeat) were exhaustively covered in the **Unit II Study Guide (`02_SECOND-CHAPTER.pdf`)**. To maintain absolute fidelity to the source materials, this document provides complete examination notes for the entirety of `06_SIXTH-CHAPTER.pdf`.

---

## 2. Introduction to Network Security & CIA Triad

**Network Security** consists of the policies, hardware systems, and software protocols implemented to protect a communication network and its underlying data from unauthorized access, risk, breaches, intrusions, and malicious threats. Network security is not merely concerned with securing individual endpoints at either end of a communication channel; it aims to ensure that the entire communication chain and network infrastructure remain secure.

The most fundamental everyday example of network security is **password protection**, where users select secret authentication credentials to restrict access to their network accounts and resources.

### The Primary Pillars of Network Security: The CIA Triad

The primary objective of network security is built upon three foundational pillars, collectively known as the **CIA Triangle** or **CIA Triad**:

1. **Confidentiality**:
   - Assures that private or sensitive information is not disclosed or made available to unauthorized persons, entities, or processes.
   - Ensures that data access is restricted strictly to intended and authorized individuals.
   - Prevents eavesdropping, illegal interception, and data exposure breaches.

2. **Integrity**:
   - Assures the accuracy, completeness, and consistency of data throughout its lifecycle.
   - Guarantees that information and system programs are modified only in a specified, authorized manner.
   - Ensures data remains reliable and safeguards against unauthorized tampering, deletion, or insertion.

3. **Availability**:
   - Assures that network data, computing resources, and communication services are continuously accessible to legitimate users whenever needed.
   - Guarantees prompt system response and prevents denial of service to authorized requests.
   - Ensures timely and dependable access to critical information infrastructure.

**[📌 REFER TO PDF DIAGRAM: Elements of Network Security (CIA Triangle) – PDF PAGE NO. 02]**

**[📌 REFER TO PDF DIAGRAM: CIA Model – PDF PAGE NO. 03]**

---

## 3. Symmetric Key Cryptography

**Symmetric Key Cryptography** (also referred to as **symmetric encryption** or secret-key encryption) is an encryption technique in which the **same secret key** is utilized for both encrypting the plaintext at the sender end and decrypting the ciphertext at the receiver end.

In symmetric encryption, the sender transforms plaintext $P$ into ciphertext $C$ using secret key $K$, and the receiver reverses the operation using the exact same secret key $K$:

$$C = E_K(P)$$
$$P = D_K(C)$$

### Fundamental Requirements of Symmetric Cryptography
- **Key Secrecy**: The shared key must be kept strictly secret between communicating parties. If an unauthorized third party discovers the key, all past and future encrypted messages are compromised.
- **Prior Key Distribution**: The sender and receiver must securely agree upon and establish the shared secret key prior to initiating encrypted communication.

**[📌 REFER TO PDF DIAGRAM: Symmetric Key Cryptography – PDF PAGE NO. 06]**

**[📌 REFER TO PDF DIAGRAM: Symmetric Encryption Flow – PDF PAGE NO. 07]**

### Classification of Symmetric Ciphers: Block Cipher vs Stream Cipher

Symmetric encryption algorithms are categorized into two structural classes based on how they process plaintext data:

1. **Block Cipher**:
   - Processes and encrypts plaintext in fixed-size blocks (e.g., 64-bit blocks in DES or 128-bit blocks in AES) using a complex encryption function.
   - Incoming data is stored in system memory until a full block is accumulated before encryption occurs.
   - Uses the same secret key to transform every block belonging to the same document.
   - Security depends entirely on the design and mathematical complexity of the block encryption function.

2. **Stream Cipher**:
   - Encrypts continuous streams of plaintext data in small units, typically bit-by-bit or byte-by-byte.
   - Data is encrypted continuously as it streams, without requiring buffering or storage in system memory.
   - Applies a continuously changing key stream generated by a pseudorandom key stream generator.
   - Security depends on the randomness and design of the key stream generator.

---

## 4. Data Encryption Standard (DES)

The **Data Encryption Standard (DES)** is a classic symmetric-key block cipher developed to protect electronic data. DES operates on plaintext divided into fixed blocks, producing ciphertext blocks of equal size.

### Key Parameters and Bit Specifications of DES
- **Plaintext Block Size**: 64 bits.
- **Ciphertext Block Size**: 64 bits.
- **Input Key Length**: 64 bits.
- **Effective Secret Key Length**: **56 bits**.
- **Parity Bits**: Every 8th bit of the 64-bit input key (8 bits total: bits 8, 16, 24, 32, 40, 48, 56, 64) is a parity check bit that is discarded before the DES encryption process begins, leaving a 56-bit key used for subkey generation.

**[📌 REFER TO PDF DIAGRAM: Encryption and Decryption with DES – PDF PAGE NO. 08]**

### General Structure and Steps in DES

DES processing follows a Feistel structure consisting of an Initial Permutation, 16 processing rounds, and a Final Permutation:

1. **Initial Permutation (IP)**:
   - The 64-bit plaintext block is handed over to the Initial Permutation function.
   - IP reorders and shuffles the 64 bits using permutation boxes (**P-boxes**), changing bit positions without altering bit values.
2. **Division into Halves**:
   - IP splits the permuted 64-bit block into two equal 32-bit halves: **Left Plain Text (LPT / $L_0$)** and **Right Plain Text (RPT / $R_0$)**.
3. **16 Encryption Rounds**:
   - Each LPT and RPT pair passes through **16 iterative rounds** of substitution and transposition.
   - **Round Key Generator**: Generates sixteen 48-bit subkeys ($K_1, K_2, \dots, K_{16}$) from the 56-bit key. The 56-bit key is split into two 28-bit halves, and each half is circularly shifted left by 1 or 2 positions depending on the round number (shifted by 1 position in rounds 1, 2, 9, and 16; shifted by 2 positions in all other rounds).
   - **Round Operation**: Each round $i$ contains two internal components:
     - **Mixer**: The 32-bit right block $R_{i-1}$ is expanded and mixed with 48-bit subkey $K_i$ via round function $F(R_{i-1}, K_i)$, and the result is XORed with 32-bit left block $L_{i-1}$ to produce new right block $R_i$.
     - **Swapper**: The right block $R_{i-1}$ becomes the new left block $L_i$ ($L_i = R_{i-1}$), while $R_i = L_{i-1} \oplus F(R_{i-1}, K_i)$.
4. **Rejoining and Final Permutation (FP)**:
   - After completing Round 16, the final LPT and RPT blocks ($L_{16}$ and $R_{16}$) are rejoined.
   - **Final Permutation (FP)** (which is the exact inverse mathematical operation of IP) is performed on the combined 64-bit block.
5. **Output**: Produces the final 64-bit ciphertext block.

**[📌 REFER TO PDF DIAGRAM: Steps in DES – PDF PAGE NO. 08]**

**[📌 REFER TO PDF DIAGRAM: Rounds in DES – PDF PAGE NO. 11]**

---

## 5. Advanced Encryption Standard (AES)

The **Advanced Encryption Standard (AES)** was created to replace DES. DES suffered from two primary weaknesses: its 56-bit key size was vulnerable to exhaustive key-search (brute force) attacks, and its hardware-oriented design ran relatively slowly in software. AES provides significantly faster performance, stronger cryptographic resistance, and flexible key sizes.

### Fundamental Characteristics of AES
- **Block Cipher Architecture**: AES is an **iterative, non-Feistel block cipher** based on a **substitution-permutation network**.
- **Fixed Block Size**: Processes plaintext in fixed blocks of **128 bits** (16 bytes).
- **Variable Key Sizes and Rounds**: The number of encryption rounds ($N_R$) depends directly on the cipher key length:

| Round Count ($N_R$) | Cipher Key Size | Block Size |
| :--- | :--- | :--- |
| **10 Rounds** | **128 bits** (16 bytes) | 128 bits |
| **12 Rounds** | **192 bits** (24 bytes) | 128 bits |
| **14 Rounds** | **256 bits** (32 bytes) | 128 bits |

**[📌 REFER TO PDF DIAGRAM: Basic Structure of AES – PDF PAGE NO. 13]**

### Byte-Level State Matrix Representation

Unlike DES, which operates on bit strings, AES performs all computations on **bytes**. It treats the 128-bit (16-byte) plaintext block as a $4 \times 4$ matrix of bytes called the **State Matrix**:

$$\begin{bmatrix} b_0 & b_4 & b_8 & b_{12} \\ b_1 & b_5 & b_9 & b_{13} \\ b_2 & b_6 & b_{10} & b_{14} \\ b_3 & b_7 & b_{11} & b_{15} \end{bmatrix}$$

### Four Sub-processes of an AES Encryption Round

Except for the final round (which omits the MixColumns step), every standard AES encryption round processes the State Matrix through four sequential sub-transformations:

1. **SubBytes (Byte Substitution)**:
   - Non-linear byte substitution where each of the 16 bytes in the State matrix is independently replaced by a corresponding byte from a static $16 \times 16$ lookup table called the **S-Box**.
   - Result is a transformed $4 \times 4$ State matrix.
2. **ShiftRows (Row Permutation)**:
   - Executes circular left shifts on the last three rows of the State matrix:
     - **Row 0**: Not shifted (0 byte shift).
     - **Row 1**: Circularly shifted left by **1 byte position**.
     - **Row 2**: Circularly shifted left by **2 byte positions**.
     - **Row 3**: Circularly shifted left by **3 byte positions**.
3. **MixColumns (Column Transformation)**:
   - Operates on the State column-by-column. Each column of 4 bytes is treated as a 4-term polynomial over $GF(2^8)$ and multiplied by a fixed matrix.
   - Takes 4 bytes of a column and outputs 4 completely new bytes to replace the original column.
   - *Note: MixColumns is omitted in the final round ($N_R$).*
4. **AddRoundKey (Key Addition)**:
   - The 128 bits (16 bytes) of the State matrix are XORed with 128 bits of the current round subkey generated by the Key Expansion algorithm.
   - In the final round, the output of AddRoundKey is the final 128-bit ciphertext.

**[📌 REFER TO PDF DIAGRAM: Round 1 Process in AES – PDF PAGE NO. 15]**

### AES Decryption Operations
AES decryption reverses the encryption process by applying inverse transformations in reverse order:
1. **AddRoundKey** (using round subkeys in reverse order).
2. **Inverse MixColumns**.
3. **Inverse ShiftRows**.
4. **Inverse SubBytes**.

---

## 6. Asymmetric Key Cryptography (Public Key Cryptography)

**Asymmetric Key Cryptography** (commonly termed **Public Key Cryptography**) solves the fundamental key distribution problem of symmetric cryptography by utilizing a mathematically linked **pair of keys**:

1. **Public Key**: Shared openly and made accessible to anyone on the network. Used by the sender to **encrypt** messages or by receivers to verify signatures.
2. **Private Key**: Kept strictly secret by the key owner. Used by the owner to **decrypt** ciphertext or generate digital signatures.

**[📌 REFER TO PDF DIAGRAM: Asymmetric Key Cryptography – PDF PAGE NO. 19]**

### Core Properties and Principles
- **Dual-Key Mechanism**: Different keys are used for encryption and decryption.
- **Unique Private Decryption**: Each receiver possesses a unique private key that cannot be derived from the corresponding public key.
- **Computational Irreversibility**: The encryption algorithm is mathematically complex enough to make it computationally infeasible for an attacker to calculate the private key or plaintext from the public key and ciphertext.

### Strategic Advantages of Public Key Cryptography
- **Enhanced Security**: Eliminates the need to transmit secret keys across insecure communication channels.
- **User Authentication**: Validates the sender's identity beyond doubt.
- **Non-Repudiation**: Prevents senders from denying having sent a message.
- **Simplified Key Management**: Scalable key distribution in large distributed networks.

---

## 7. RSA Algorithm & Step-by-Step Solved Example

The **RSA Algorithm** (named after its inventors **Ron Rivest, Adi Shamir, and Leonard Adleman**) is the most widely adopted public key encryption algorithm. Its cryptographic strength relies on the extreme computational difficulty of factoring the product of two large prime numbers.

### Step-by-Step RSA Algorithm Procedure

1. **Select Primes**: Choose two distinct large prime numbers $p$ and $q$.
2. **Calculate Modulus**: Compute $n = p \times q$. The value $n$ represents the modulus for both public and private keys.
3. **Calculate Totient**: Compute Euler's totient function:
   $$\phi(n) = (p - 1) \times (q - 1)$$
4. **Select Public Exponent $e$**: Choose integer $e$ such that $1 < e < \phi(n)$ and $e$ is coprime to $\phi(n)$ ($\gcd(e, \phi(n)) = 1$).
   - **Public Key**: Pair $(e, n)$.
5. **Calculate Private Exponent $d$**: Determine $d$ such that $(d \times e) \bmod \phi(n) = 1$, which is calculated using:
   $$d = \frac{1 + (k \times \phi(n))}{e}$$
   where $k$ is an arbitrary positive integer chosen to make $d$ an integer.
   - **Private Key**: Pair $(d, n)$.
6. **Encryption**: Convert plaintext into integer $M$ ($0 \le M < n$). Compute ciphertext $C$:
   $$C = M^e \bmod n$$
7. **Decryption**: Compute original plaintext $M$ from ciphertext $C$:
   $$M = C^d \bmod n$$

**[📌 REFER TO PDF DIAGRAM: RSA Algorithm – PDF PAGE NO. 21]**

---

### Step-by-Step Solved Numerical Example (From PDF)

#### Given Problem Statement:
Perform RSA encryption and decryption using prime numbers $p = 5$ and $q = 11$, with public exponent $e = 7$ and plaintext message $M = 15$.

#### Step-by-Step Solution:

1. **Given Prime Numbers**:
   $$p = 5, \quad q = 11$$

2. **Calculate Modulus $n$**:
   $$n = p \times q = 5 \times 11 = 55$$

3. **Calculate Euler's Totient Function $\phi(n)$**:
   $$\phi(n) = (p - 1) \times (q - 1) = (5 - 1) \times (11 - 1) = 4 \times 10 = 40$$

4. **Establish Public Key $(e, n)$**:
   - Select $e = 7$.
   - Verify conditions: $1 < 7 < 40$ and $\gcd(7, 40) = 1$ (since 7 and 40 share no common factors).
   - **Public Key**: $(e, n) = (7, 55)$.

5. **Encryption Process**:
   - Given Plaintext Message $M = 15$.
   - Apply RSA Encryption Formula:
     $$C = M^e \bmod n = 15^7 \bmod 55$$
   - Calculating $15^7$:
     $$15^7 = 170,859,375$$
   - Divide by 55 to find remainder:
     $$170,859,375 \div 55 = 3,106,534.090909...$$
     $$3,106,534 \times 55 = 170,859,370$$
     $$C = 170,859,375 - 170,859,370 = 5$$
   - **Resulting Ciphertext**: $C = 5$.

6. **Calculate Private Key $(d, n)$**:
   - Formula for private key exponent $d$:
     $$d = \frac{1 + (k \times \phi(n))}{e}$$
   - Testing values for integer $k$:
     - For $k = 1$: $d = \frac{1 + 40}{7} = \frac{41}{7}$ (not an integer)
     - For $k = 2$: $d = \frac{1 + 80}{7} = \frac{81}{7}$ (not an integer)
     - For $k = 3$: $d = \frac{1 + 120}{7} = \frac{121}{7}$ (not an integer)
     - For $k = 4$: $d = \frac{1 + (4 \times 40)}{7} = \frac{1 + 160}{7} = \frac{161}{7} = 23$
   - **Private Key**: $(d, n) = (23, 55)$.

7. **Decryption Process**:
   - Apply RSA Decryption Formula using $C = 5$ and private key $d = 23$:
     $$M = C^d \bmod n = 5^{23} \bmod 55$$
   - Using modular exponentiation:
     $$5^1 \bmod 55 = 5$$
     $$5^2 \bmod 55 = 25$$
     $$5^3 \bmod 55 = 125 \bmod 55 = 15$$
     $$5^4 \bmod 55 = (15 \times 5) \bmod 55 = 75 \bmod 55 = 20$$
     $$5^5 \bmod 55 = (20 \times 5) \bmod 55 = 100 \bmod 55 = 45$$
     $$5^6 \bmod 55 = (45 \times 5) \bmod 55 = 225 \bmod 55 = 5$$
   - Since $5^6 \equiv 5 \pmod{55}$, notice that $5^{23} = (5^6)^3 \times 5^5$:
     $$5^{23} \bmod 55 = (5^3 \times 45) \bmod 55 = (15 \times 45) \bmod 55 = 675 \bmod 55 = 15$$
   - **Recovered Plaintext**: $M = 15$.

The decrypted plaintext matches the original input message ($M = 15$), proving algorithm accuracy.

---

## 8. User Authentication & Hash Functions

### User Authentication Architecture

**Authentication** is the process of verifying the identity of a user, process, or system before granting access to computer systems or network resources. User authentication systems are classified into three levels based on security factors:

1. **Single-Factor Authentication**:
   - First authentication method developed.
   - Relies solely on a single factor: **username and password**.
   - If credentials match system records, access is granted; otherwise access is denied.
2. **Two-Factor Authentication (2FA)**:
   - Requires two distinct security factors: username/password plus a secondary physical or digital token.
   - Examples: One-Time Passwords (OTP) sent via SMS, wireless authentication tokens, virtual token applications.
3. **Multi-Factor Authentication (MFA)**:
   - Requires two or more independent authentication factors (knowledge factor like password, possession factor like hardware key/phone, inherence factor like biometrics).
   - Highly resilient against keylogger spyware and phishing attacks.

### Hash Functions & Message Digests

A **Hash Function** is a mathematical algorithm that converts an arbitrary variable-length input message $M$ into a unique, fixed-length output string called a **hash value** or **Message Digest** (typically expressed in hexadecimal notation).

$$h = H(M)$$

#### Core Properties of Cryptographic Hash Functions
- **Fixed-Length Output**: Regardless of whether the input is a single character or a multi-gigabyte file, the output hash value remains fixed in size.
- **Avalanche Effect**: A minute change in the input message (e.g., modifying a single bit) causes a radical, unpredictable change in the output hash value. The hash value of block 1 feeds into block 2, cascading across all subsequent operations.

**[📌 REFER TO PDF DIAGRAM: Hash Function Concept – PDF PAGE NO. 26]**

**[📌 REFER TO PDF DIAGRAM: Design of Hashing Algorithm – PDF PAGE NO. 27]**

### Secure Hash Algorithm (SHA) & Java Implementation

The **Secure Hash Algorithm (SHA)** is a family of cryptographic hash functions developed as a stronger alternative to MD5. SHA compresses input data into shorter digests using bitwise logical operations, modular additions, and compression functions.

In Java, cryptographic hash values are calculated using the `MessageDigest` class under the `java.security` package:

- **Supported Algorithms**: `MD2`, `MD5`, `SHA-1`, `SHA-224`, `SHA-256`, `SHA-384`, `SHA-512`.
- **Implementation Workflow**:
  1. Initialize static instance: `MessageDigest md = MessageDigest.getInstance("SHA-256");`
  2. Compute digest bytes from input text byte array: `byte[] digest = md.digest(inputBytes);`
  3. Convert byte array into signum representation using `BigInteger`: `BigInteger no = new BigInteger(1, digest);`
  4. Format `BigInteger` into a hexadecimal string representation to obtain the final Message Digest.

---

## 9. Digital Signatures

A **Digital Signature** is a mathematical cryptographic value calculated from the message data and a secret private key known only to the signer. It binds a document to the identity of its creator, providing non-repudiation, message integrity, and origin authentication.

### Characteristics of Digital Signatures
- **Authenticity**: Proves beyond doubt that the message was created by the holder of the private key.
- **Integrity**: Guarantees that the message content was not modified in transit.
- **Transportability**: Easily attached to electronic documents and transmitted over public networks.
- **Automatic Time-Stamping**: Can be time-stamped to prove when the signature was created.

### Digital Signature Workflow

**[📌 REFER TO PDF DIAGRAM: Digital Signature Process – PDF PAGE NO. 32]**

#### 1. Operations at Sender End:
1. Sender runs the original message data through a hash algorithm to compute a **Message Digest (Hash Value)**.
2. Sender encrypts the Message Digest using the **Sender's Private Key**. The resulting encrypted digest is the **Digital Signature**.
3. Sender attaches the Digital Signature to the message data and transmits both to the receiver.

#### 2. Verification Operations at Receiver End:
1. Receiver detaches the Digital Signature and decrypts it using the **Sender's Public Key** to extract the original Message Digest sent by the sender.
2. Receiver passes the received message data through the exact same hash algorithm to compute a new **Receiver Message Digest**.
3. Receiver compares the decrypted Sender Message Digest with the freshly computed Receiver Message Digest:
   - **If Digests are Equal**: The signature is valid, confirming authentic sender identity and complete message integrity.
   - **If Digests Differ**: The signature is invalid, indicating data tampering or an forged sender identity.

---

## 10. Firewalls & Network Perimeter Security

A **Firewall** is a network perimeter security system that monitors, filters, and controls incoming and outgoing network traffic according to predefined security rules.

### Firewall Principles and Deployment
- **Form Factor**: Can be implemented as hardware appliances, software applications, or hybrid combinations.
- **Mandatory Perimeter Inspection**: All traffic entering or exiting the protected internal network must pass through the firewall.
- **Rule Enforcement**: The firewall inspects every packet and accepts or rejects traffic based on local security policies.

**[📌 REFER TO PDF DIAGRAM: Firewall Architecture – PDF PAGE NO. 34]**

### Primary Types of Firewalls

1. **Packet Filtering Firewall**:
   - Inspects network traffic at the Network and Transport layers (IP/TCP headers).
   - Applies filtering rules to each incoming or outgoing IP packet to determine whether to **forward** or **discard**.
   - Filtering parameters include:
     - **Source IP Address**: IP address of origin host.
     - **Destination IP Address**: IP address of target host.
     - **Interface**: Router interface port where packet arrived or is destined.
     - **Protocol**: Protocol type (e.g., TCP, UDP, ICMP).
     - **Port Numbers**: Source and destination application ports (e.g., Port 80 for HTTP, Port 22 for SSH).

**[📌 REFER TO PDF DIAGRAM: Packet Filtering Firewall – PDF PAGE NO. 36]**

2. **Application Level Gateway (Proxy Server)**:
   - Operates at the Application Layer, acting as an intermediary (proxy) for specific application protocols (TELNET, FTP, SMTP, HTTP).
   - An internal user contacts the application gateway using client software.
   - The gateway authenticates user credentials, prompts for target remote host details, establishes a separate connection to the remote host, and relays TCP application data between endpoints.

**[📌 REFER TO PDF DIAGRAM: Proxy Server (Application Level Gateway) – PDF PAGE NO. 37]**

3. **Circuit Level Gateway**: Operates at the transport layer, validating TCP handshakes before establishing circuit connections.
4. **Stateful Packet Filtering Firewall**: Tracks the active state of TCP connections in state tables to permit response traffic.
5. **Next-Generation Firewall (NGFW)**: Combines traditional packet inspection with deep packet inspection (DPI), intrusion prevention (IPS), and application control.

---

## 11. Important Definitions & Comparative Summary Tables

### Key Technical Definitions

- **CIA Triad**: The three core objectives of information security—Confidentiality, Integrity, and Availability.
- **Symmetric Encryption**: Cryptographic scheme utilizing a single secret key for both encryption and decryption.
- **Block Cipher**: An encryption method that processes data in fixed-size blocks (e.g., 64-bit or 128-bit blocks).
- **Stream Cipher**: An encryption method that continuously processes plaintext bit-by-bit or byte-by-byte using a pseudorandom key stream.
- **Data Encryption Standard (DES)**: A 64-bit block cipher utilizing a 56-bit secret key across 16 Feistel rounds.
- **Advanced Encryption Standard (AES)**: A non-Feistel, 128-bit block cipher supporting 128-bit, 192-bit, or 256-bit key sizes.
- **Public Key Cryptography**: An asymmetric encryption scheme using a public key for encryption and a private key for decryption.
- **RSA Algorithm**: An asymmetric cryptographic algorithm based on the difficulty of factoring large composite prime numbers.
- **Message Digest**: A fixed-length output string produced by passing an arbitrary-length message through a cryptographic hash function.
- **Digital Signature**: An encrypted hash value created with a sender's private key that provides origin authentication and message integrity.
- **Packet Filtering Firewall**: A perimeter defense device that filters IP packets based on network layer headers and port numbers.
- **Proxy Server**: An application-level firewall gateway that authenticates application sessions and relays data between internal and external hosts.

---

### Comparative Analysis Matrices

#### Table 1: Symmetric vs Asymmetric Cryptography

| Feature / Criterion | Symmetric Key Cryptography | Asymmetric Key Cryptography |
| :--- | :--- | :--- |
| **Number of Keys** | Single shared secret key | Pair of keys (Public Key & Private Key) |
| **Key Distribution** | Difficult (requires secure out-of-band channel) | Simple (Public Key shared freely) |
| **Execution Speed** | Very fast (computationally efficient) | Slower (complex modular arithmetic) |
| **Primary Algorithms** | DES, AES, 3DES, Blowfish | RSA, ECC, Diffie-Hellman |
| **Resource Usage** | Low memory and CPU overhead | High mathematical processing overhead |

---

#### Table 2: Data Encryption Standard (DES) vs Advanced Encryption Standard (AES)

| Property / Feature | Data Encryption Standard (DES) | Advanced Encryption Standard (AES) |
| :--- | :--- | :--- |
| **Block Size** | 64 bits | 128 bits |
| **Key Lengths** | 56 bits (8 parity bits discarded from 64-bit input) | 128 bits, 192 bits, or 256 bits |
| **Number of Rounds** | 16 rounds | 10 rounds (128-bit), 12 rounds (192-bit), 14 rounds (256-bit) |
| **Structure Type** | Feistel cipher network | Non-Feistel Substitution-Permutation Network |
| **Security Level** | Vulnerable to brute force attacks | Highly secure (current global standard) |
| **Processing Unit** | Bit-level manipulation | Byte-level state matrix operations |

---

#### Table 3: Block Cipher vs Stream Cipher

| Feature / Criterion | Block Cipher | Stream Cipher |
| :--- | :--- | :--- |
| **Processing Unit** | Fixed-size data blocks (64 bits, 128 bits) | Continuous bit-by-bit or byte-by-byte stream |
| **Memory Buffer** | Requires system buffer to store complete block | Minimal memory required (real-time processing) |
| **Key Usage** | Same key encrypts entire data block | Continuously changing key stream generator |
| **Examples** | DES, AES | RC4, A5/1 |

---

#### Table 4: Packet Filtering Firewall vs Application Level Gateway (Proxy Server)

| Feature / Criterion | Packet Filtering Firewall | Application Level Gateway (Proxy Server) |
| :--- | :--- | :--- |
| **OSI Layer** | Network and Transport Layers (Layers 3 & 4) | Application Layer (Layer 7) |
| **Inspection Scope** | Header fields (Source IP, Dest IP, Ports, Protocol) | Full payload and application protocol commands |
| **Performance Speed** | Extremely fast (low latency) | Slower (requires deep application proxying) |
| **User Authentication** | Does not authenticate individual users | Authenticates user credentials before relaying |

---

## 12. Quick-Reference List of PDF Diagrams

For quick reference during university examination preparation, the key diagrams from `06_SIXTH-CHAPTER.pdf` are indexed below:

1. **Elements of Network Security (CIA Triangle)** — PDF Page 02
2. **CIA Model Architecture** — PDF Page 03
3. **Symmetric Key Cryptography Structure** — PDF Page 06
4. **Symmetric Encryption Process** — PDF Page 07
5. **Encryption and Decryption with DES** — PDF Page 08
6. **Overall Steps in DES Cipher** — PDF Page 08
7. **Round Details in DES (Mixer & Swapper)** — PDF Page 11
8. **Basic Structure of AES Cipher** — PDF Page 13
9. **Round 1 Sub-processes in AES** — PDF Page 15
10. **Asymmetric Key Cryptography (Public Key)** — PDF Page 19
11. **RSA Algorithm Architecture** — PDF Page 21
12. **Hash Function Concept** — PDF Page 26
13. **Design of Hashing Algorithm** — PDF Page 27
14. **Digital Signature Process (Sender & Receiver)** — PDF Page 32
15. **Perimeter Firewall System** — PDF Page 34
16. **Packet Filtering Firewall Architecture** — PDF Page 36
17. **Proxy Server (Application Level Gateway)** — PDF Page 37

---

## 13. Chapter Summary

Unit VI establishes the essential principles of network security and cryptographic protection:

- **Security Pillars**: Network security rests on the **CIA Triad**—**Confidentiality** (disclosures restricted to authorized users), **Integrity** (data accuracy and protection against tampering), and **Availability** (uninterrupted access to network services).
- **Symmetric Cryptography**: Employs a single secret key for encryption and decryption. **DES** processes 64-bit blocks using a 56-bit key across 16 Feistel rounds. **AES** overcomes DES key vulnerability by using 128-bit blocks with 128, 192, or 256-bit keys across 10, 12, or 14 rounds of SubBytes, ShiftRows, MixColumns, and AddRoundKey.
- **Asymmetric Cryptography**: Uses a dual-key pair (Public Key for encryption/verification, Private Key for decryption/signing). **RSA** relies on prime factorization hardness ($n = p \times q$, $\phi(n) = (p-1)(q-1)$, $C = M^e \bmod n$, $M = C^d \bmod n$).
- **Integrity and Authentication**: **Hash functions** (SHA family) generate fixed-length message digests showcasing the avalanche effect. **Digital signatures** encrypt message digests with the sender's private key, enabling receivers to verify authenticity using the sender's public key. **Multi-Factor Authentication (MFA)** combines independent factors to prevent unauthorized login.
- **Perimeter Defense**: **Firewalls** monitor traffic based on security policies. **Packet Filtering Firewalls** evaluate IP/TCP headers, while **Application Level Gateways (Proxy Servers)** proxy application protocols and authenticate sessions.
