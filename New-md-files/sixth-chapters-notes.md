# CHAPTER 6: NETWORK SECURITY

---

## Module 1: Introduction to Network Security, CIA Triad & Symmetric Key Cryptography

### Topic A: Overview & Elements of Network Security (The CIA Triad)

**Question:** *Define Network Security and explain the elements of the CIA Triad.* **[4 Marks]**

**1. Definition of Network Security:**

- **Network security** is the security provided to a network from unauthorized access and risk.
- It protects the network and data from **breaches, intrusions, and other threats**.
- Computer networks and data can be secured using a combination of **hardware and software systems**.
- Network security is not only concerned about the security of the computers at each end of the communication chain; it aims to ensure that the **entire network is secure**.
- The most basic everyday example of network security is **password protection**, which the user of the network chooses for themselves.

**2. Elements of Network Security (The CIA Model):** The primary goals of network security are **Confidentiality, Integrity, and Availability**, commonly represented as the **CIA Triangle / CIA Model**:

```text
                     [ Confidentiality ]
                     /                 \
                    /   Information     \
                   /   kept private      \
                  /     and secure        \
                 /                         \
    [ Integrity ] ------------------------- [ Availability ]
   (Data not modified,                     (Systems & data available
    deleted, or added)                      to legitimate users)
```

1. **Confidentiality:**
    - Assures that private or confidential information is not made available or disclosed to unauthorized persons.
    - It ensures that data is accessible **only to intended and authorized persons**.
2. **Integrity:**
    - Assures the **accuracy and consistency** of data.
    - Assures that information and programs are changed only in a specified and authorized manner.
    - It ensures that the data is reliable and is **not changed, modified, deleted, or added by unauthorized persons**.
3. **Availability:**
    - Assures that data and network resources/services are **continuously available to legitimate users** whenever they require them.
    - Assures that systems work promptly and service is not denied to authorized users, ensuring timely and reliable access to and use of information.

### Topic B: Symmetric Key Cryptography

**1. Concept and Mechanism:**

- **Symmetric key cryptography** (also called **symmetric encryption**) is a type of encryption technique in which the **same key** is used for both encryption and decryption.
- The same key is used by the sender for encrypting original data and by the receiver for decrypting the scrambled data.
- **Two Strict Rules of Symmetric Cryptography:**
    1. This key must be **kept strictly secret**.
    2. The sender and receiver **must agree on a key before they communicate**.

**2. Standard Examination Diagram:** *(Reference: `06_SIXTH-CHAPTER.pdf`, Page 06 & Page 07)*

```text
               <------------------- Sender -------------------> | <------------------ Receiver ------------------>
                                                                |
               +------------+                                   |                                   +------------+
               |  Original  |                                   |                                   |  Original  |
               | Plaintext  |                                   |                                   | Plaintext  |
               |  Message   |                                   |                                   |  Message   |
               +-----+------+                                   |                                   +-----+------+
                     |                                          |                                         ^
                     v                                          |                                         |
               +-----+------+                                   |                                   +-----+------+
               | Encryption |                                   |                                   | Decryption |
               | Algorithm  |                                   |                                   | Algorithm  |
               +-----+------+                                   |                                   +-----+------+
                     ^                                          |                                         ^
                     |                                          |                                         |
           [ Secret Key Shared ]                                |                               [ Secret Key Shared ]
           [ by Sender/Receiver]                                |                               [ by Sender/Receiver]
                     |                                          |                                         |
                     v                                          |                                         |
               +-----+------+                                   |                                         |
               | Scrambled  |                                   |                                         |
               | Ciphertext | ================= (Transmitted across Network) =============================+
               +------------+
```

**3. Types of Symmetric Key Cryptography:** Symmetric key cryptography is classified into two distinct types based on how plaintext is processed:

#### 1) Block Cipher

- A block cipher encrypts a **fixed block of data** (such as **64-bit blocks** of data) with a complex encryption function.
- A set number of bits are encrypted in electronic data blocks using a particular secret key.
- As data is encrypted, the system **keeps the data in its memory** as it waits for a full block to accumulate.
- A block cipher encrypts blocks belonging to the same document **all under the same key**.
- The security of these ciphers depends totally on the **design and complexity of the encryption function**.
- *(Example: DES - Data Encryption Standard)*

#### 2) Stream Cipher

- A stream cipher encrypts smaller blocks of plaintext data, usually **bit-by-bit or byte-by-byte**.
- The data is **encrypted as it streams** rather than being stored in system memory.
- It encrypts the plaintext under a **continuously changing key stream**.
- The security of stream ciphers depends entirely on the **design of the key stream generator**.

### 4. Comparative Summary Table (Block Cipher vs. Stream Cipher)

| Parameter / Feature | Block Cipher | Stream Cipher |
| --- | --- | --- |
| **Data Processing Unit** | Encrypts fixed-size blocks (e.g., 64-bit blocks). | Encrypts small units continuously (bit-by-bit or byte-by-byte). |
| **System Memory Usage** | Stores data in memory while waiting for a complete block. | Encrypts on the fly as data streams; not stored in memory. |
| **Key Application** | The same key encrypts all blocks belonging to the document. | Uses a continuously changing key stream. |
| **Security Dependence** | Depends on the design of the encryption function. | Depends on the design of the key stream generator. |
| **Standard Example** | DES (Data Encryption Standard). | Continuous stream generators. |

---

## Module 2: Data Encryption Standard (DES)

### A. Overview & Key Specifications

- **DES (Data Encryption Standard)** is a **symmetric key block cipher** that is widely used to secure digital data.
- In DES, plaintext is divided into blocks (hence called a block cipher).
- **Block Size:** Takes a **64-bit plaintext** input and produces a **64-bit ciphertext** output.
- **Decryption:** Reverses the process using a DES reverse cipher, taking 64-bit ciphertext as input to regenerate the original 64-bit plaintext.
- **Key Length:**
    - Total input key = **64 bits**.
    - Every 8th bit (8 bits total) is a **parity bit** used for error checking and is discarded before the process begins.
    - **Effective Secret Key = 56 bits**.
- **Core Operations:** In each round, DES relies on two fundamental operations:
    1. **Substitution**
    2. **Transposition (Permutation)**

### B. Standard Exam Diagram 1: High-Level DES Encryption & Decryption

```text
             ENCRYPTION                                              DECRYPTION
        64-bit Plaintext                                        64-bit Plaintext
               |                                                       ^
               v                                                       |
        +--------------+                                        +--------------+
        |  DES Cipher  | <---------- 56-bit Key --------------> | DES Reverse  |
        +--------------+                                        |    Cipher    |
               |                                                +--------------+
               v                                                       ^
        64-bit Ciphertext                                              |
               \                                                       /
                \================= (Network) =========================/
```

### C. General Structure & Step-by-Step Flow of DES

```text
                         +-----------------------+
                         |  Plaintext (64-bit)   |
                         +-----------+-----------+
                                     |
                                     v
                         +-----------------------+
                         |  Initial Permutation  |
                         |         (IP)          |
                         +-----------+-----------+
                                     |
                               +-----+-----+
                               |           |
                               v           v
                            +-----+     +-----+
                            | LPT |     | RPT |  (32 bits each)
                            +--+--+     +--+--+
                               |           |
                               v           v
                         +-----------+-----------+
                         |       16 Rounds       | <==== 48-bit Subkeys
                         +-----------+-----------+
                               |           |
                               +-----+-----+
                                     |
                                     v
                         +-----------------------+
                         |   Final Permutation   |
                         |         (FP)          |
                         +-----------+-----------+
                                     |
                                     v
                         +-----------------------+
                         | Ciphertext (64-bit)   |
                         +-----------------------+
```

#### Step-by-Step Explanation

1. **Initial Permutation (IP):**
    - The process begins by handing the 64-bit plaintext block to the **Initial Permutation (IP)** function.
    - IP uses **P-boxes** (Permutation boxes) to **shuffle or transpose the bits**, meaning the location of the bits is changed.
2. **Splitting into Halves:**
    - IP divides the permuted 64-bit block into two equal 32-bit halves:
        - **LPT (Left Plain Text)** = 32 bits
        - **RPT (Right Plain Text)** = 32 bits
3. **16 Encryption Rounds:**
    - Each LPT and RPT passes through **16 iterative rounds** of encryption.
4. **Rejoining and Final Permutation (FP):**
    - After round 16, LPT and RPT are rejoined into a 64-bit block.
    - The **Final Permutation (FP)** is executed on this combined block.
5. **Output:** Produces the final desired **64-bit ciphertext**.

### D. Detailed Internal Working of a Single DES Round

Each of the 16 rounds consists of two main elements:

1. **Mixer (Mixes the bits):**
    - Takes the 32-bit right block ($R_i$) and applies a round function $F(R_i, K_i)$ with the 48-bit round subkey ($K_i$).
    - The 32-bit output of this function is **XORed ($\oplus$)** with the 32-bit left block ($L_i$).
    - The output of this XOR operation becomes the new right block ($R_{i+1}$) for the next round.
2. **Swapper (Swaps the blocks):**
    - The original right block ($R_i$) simply moves across without alteration to become the new left block ($L_{i+1}$).
    - These swapped 32-bit blocks ($L_{i+1}$ and $R_{i+1}$) serve as inputs to the next round.

> **[DIAGRAM NEEDS VERIFICATION]** In the pasted notes this diagram had lost all line breaks and could not be recovered exactly. The version below is a reconstruction that uses only the components and connections named in the pasted text and in the Mixer/Swapper description above. Compare it with your teacher's diagram.

```text
        32-bits (L_i)     32-bits (R_i)
              |                 |
              |                 +-----------------------------+
              |                 |                             |
              |                 v                             |
              |        +--------+--------+                    |
              |        |   F(R_i, K_i)   | <-- K_i            |
              |        +--------+--------+ (48-bit Subkey)    |
              v                 |                             |
           [ XOR ] <------------+                             |
              |                                               |
              +-----------------------+                       |
                                      |   (SWAPPER)           |
              +-----------------------|-----------------------+
              |                       |
              v                       v
          +-------+               +-------+
          | L_i+1 |               | R_i+1 |
          +-------+               +-------+
           32-bit                  32-bit
```

> **Original pasted text of this diagram (preserved for comparison):**
>
> `32-bits (L_i) 32-bits (R_i) | | | +--------+--------+ | | | | v | | +-----------------+ | | | F(R_i, K_i) | <-- K_i| (48-bit Subkey) | +--------+--------+ | v | | [ XOR ] <------------------+ | | | +-------------------+ +-----------------+ | | (SWAPPER) v v / \ v v +-----+ +-----+ |L_i+1| |R_i+1| +-----+ +-----+ 32-bit 32-bit`

### E. Round Key Generator (How Subkeys are Generated)

- The key generator generates **16 subkeys of 48 bits each** (one for each round).
- The original 64-bit key drops its 8 parity bits, leaving a **56-bit effective key**.
- This 56-bit key is divided into **two halves of 28 bits each**.
- Both 28-bit halves are **circularly shifted left** by 1 or 2 positions depending on the round number:
    - **Shift by 1 bit:** For rounds **1, 2, 9, and 16**.
    - **Shift by 2 bits:** For all remaining rounds.
- After shifting, a compression permutation reduces the 56 bits down to a **48-bit subkey** for that round.

---

## Module 3: Advanced Encryption Standard (AES)

### 1. Introduction & Need for AES

- **Advanced Encryption Standard (AES)** is an iterative, symmetric-key block cipher developed to overcome the limitations of the older Data Encryption Standard (DES).
- **Why AES Replaced DES:** DES utilized an effective key size of only **56 bits**, making it susceptible to brute-force attacks as computational power increased. Furthermore, DES was designed for hardware implementation and ran slowly in software. AES was designed to be significantly **faster, cryptographically stronger, and more flexible**.
- **Cipher Architecture:** Unlike DES, which follows a Feistel structure, AES is a **non-Feistel cipher** based on a **Substitution-Permutation Network (SPN)**. It processes the entire block simultaneously.

### 2. Fundamental Specifications: Key Sizes and Rounds

- **Block Size:** AES processes plaintext in fixed blocks of **128 bits (16 bytes)**.
- **Variable Key Sizes & Rounds:** The number of encryption rounds ($N_r$) varies depending directly on the chosen cipher key length:

| Number of Rounds ($N_r$) | Cipher Key Size | Block Size |
| --- | --- | --- |
| 10 Rounds | 128 bits (16 bytes) | 128 bits |
| 12 Rounds | 192 bits (24 bytes) | 128 bits |
| 14 Rounds | 256 bits (32 bytes) | 128 bits |

- **Key Expansion:** The cipher key goes to a key expansion algorithm that generates round keys. It always generates **one more key than the number of rounds** (e.g., for 10 rounds, it generates **11 round keys** because one key is used in the initial pre-round transformation). Each round key is **128 bits**.

---

### 3. The Byte-Level State Matrix ($4 \times 4$ Grid)

- AES performs all internal operations on **bytes rather than bits**.
- The 128-bit (16-byte) block of input data is mapped into a $4 \times 4$ **two-dimensional array of bytes** known as the **State Matrix**.
- Bytes are populated **column-by-column**:

$$\text{State Matrix} = \begin{bmatrix} b_0 & b_4 & b_8 & b_{12} \\ b_1 & b_5 & b_9 & b_{13} \\ b_2 & b_6 & b_{10} & b_{14} \\ b_3 & b_7 & b_{11} & b_{15} \end{bmatrix}$$

---

### 4. Examination Diagram: AES Architecture & Round Process

*(Draw these two standard structural diagrams from Pages 13 and 15 of `06_SIXTH-CHAPTER.pdf`)*

**Fig (a): Overall Structure of AES**

```text
                    128-bit Plaintext
                           |
                           v
              +--------------------------+
              | Pre-Round Transformation | <--- Round Key K0 (128-bit)
              +------------+-------------+
                           |
                           v
              +--------------------------+
              |         Round 1          | <--- Round Key K1
              +------------+-------------+
                           |
                           v
              +--------------------------+
              |         Round 2          | <--- Round Key K2
              +------------+-------------+
                           |
                           v
                           :
                           v
              +--------------------------+
              |         Round Nr         | <--- Round Key Knr
              +------------+-------------+
                           |
                           v
                    128-bit Ciphertext
```

**Fig (b): Internal Round 1 Process**

```text
                    Plaintext 128-bit
                           |
                           v
                 +-------------------+
                 |   Add Round Key   | <--- K0 (128-bit Cipher Key)
                 +---------+---------+
                           |
                           v
               +-----------------------+
               |  1. SubBytes          |
               |  (Byte Substitution)  |
               +-----------+-----------+
                           |
                           v
               +-----------------------+
               |  2. ShiftRows         |
               |  (Row Permutation)    |
               +-----------+-----------+
                           |
                           v
               +-----------------------+
               |  3. MixColumns        |  <-- (Omitted in Final Round Nr)
               |  (Matrix Multiplic.)  |
               +-----------+-----------+
                           |
                           v
               +-----------------------+
               |  4. AddRoundKey       | <--- K1 (128-bit Subkey)
               +-----------+-----------+
                           |
                           v
                      Output to
                     Next Round
```

---

### 5. Detailed Working of the Four Round Transformations

Except for the final round (which omits `MixColumns`), each standard round of AES executes four sequential transformations:

#### 1. SubBytes (Byte Substitution)

- **Mechanism:** A non-linear substitution step where all 16 bytes of the State Matrix are independently substituted with another byte by looking up a fixed $16 \times 16$ table called an **S-Box (Substitution Box)**.
- **Result:** Produces a modified $4 \times 4$ State Matrix.
- **Function:** Provides **confusion**, destroying simple linear algebraic relationships.

#### 2. ShiftRows (Row Permutation)

- **Mechanism:** Operates on the State Matrix row-by-row by executing **circular left shifts** on each of the four rows:
    - **Row 0 (1st row):** **Not shifted** (0 byte shift).
    - **Row 1 (2nd row):** Circularly shifted left by **1 byte**.
    - **Row 2 (3rd row):** Circularly shifted left by **2 bytes**.
    - **Row 3 (4th row):** Circularly shifted left by **3 bytes**.
- **Result:** Rearranges the position of bytes across columns to achieve **diffusion**.

#### 3. MixColumns (Column Matrix Multiplication)

- **Mechanism:** Operates column-by-column. Each column consisting of 4 bytes is transformed using a special mathematical function (multiplication by a fixed matrix over Galois Field $GF(2^8)$).
- **Output:** Takes 4 bytes of a column and outputs 4 completely new bytes that replace the original column.
- **CRITICAL RULE:** **MixColumns is NOT present in the final round (Round $N_r$)**.

#### 4. AddRoundKey (Key Addition)

- **Mechanism:** The 16 bytes (128 bits) of the State Matrix are bitwise **XORed ($\oplus$)** with the 128-bit round key generated by key expansion.
- If it is the final round, the output of `AddRoundKey` is the final **128-bit ciphertext**; otherwise, the resulting 128 bits (16 bytes) are fed into the next round.

---

### 6. AES Decryption (Inverse Process)

To decrypt ciphertext and recover plaintext, AES applies the **inverse transformations in reverse order**:

1. **AddRoundKey** (using subkeys in reverse order)
2. **Inverse MixColumns**
3. **Inverse ShiftRows** (shifts rows to the right)
4. **Inverse SubBytes** (uses inverse S-Box)

---

## Module 4: Asymmetric Key Cryptography (Public Key Cryptography)

### 1. Why Was Asymmetric Cryptography Invented? (The Big Problem It Solves)

In **Symmetric Key Cryptography** (like DES and AES), both parties must use the **exact same secret key**.

- **The Key Distribution Problem:** How can two people on opposite sides of the internet securely agree on that secret key without someone intercepting it? If an attacker intercepts the key during transmission, all security is broken.
- **The Solution:** In 1976, **Whitfield Diffie and Martin Hellman** introduced **Asymmetric Key Cryptography** (also known as **Public Key Cryptography**). It completely eliminates the need to transmit or share secret keys.

### 2. Core Concept: The Two-Key Pair (Public Key & Private Key)

Unlike symmetric encryption, asymmetric cryptography uses a **mathematically linked pair of keys**:

```text
                       +---------------------------------------+
                       |             THE KEY PAIR              |
                       +-------------------+-------------------+
                                           |
                    +----------------------+----------------------+
                    |                                             |
                    v                                             v
          [    PUBLIC KEY    ]                          [   PRIVATE KEY   ]
          • Made public to everyone                     • Kept strictly secret
          • Published in directories                    • Stored safely by owner
          • Used to ENCRYPT data                        • Used to DECRYPT data
```

1. **Public Key:**
    - Known to everyone and freely distributed to anyone who wants to send a message.
    - Anyone can use the receiver's public key to **encrypt** a message.
2. **Private Key (Secret Key):**
    - Kept strictly secret by its owner. It is **never shared, published, or transmitted**.
    - Only the owner can use this private key to **decrypt** the ciphertext.

### 3. Mathematical One-Way Trapdoor Principle (Why It's Secure)

- **Asymmetric Property:** A message encrypted with someone's **Public Key** can **only be decrypted by their corresponding Private Key**. Even the person who encrypted the message cannot decrypt it.
- **Computational Irreversibility:** The keys are mathematically related, but the math is designed like a **one-way function (a trapdoor)**:
    - Going forward (encrypting) is extremely fast and easy.
    - Going backward (calculating the private key from the public key) is **computationally infeasible** because it requires solving extremely hard mathematical problems (such as factoring massive prime numbers).

---

### 4. Standard Examination Flow Diagram

*(Draw this clean architecture diagram from Page 19 of `06_SIXTH-CHAPTER.pdf`)*

```text
     <----------------- SENDER -----------------> | <---------------- RECEIVER ----------------->
                                                  |
     +-----------+                                |                                 +-----------+
     | Plaintext |                                |                                 | Plaintext |
     |  Message  |                                |                                 |  Message  |
     +-----+-----+                                |                                 +-----+-----+
           |                                      |                                       ^
           v                                      |                                       |
     +-----+-----+                                |                                 +-----+-----+
     |Encryption | <--- [Receiver's Public Key]   |   [Receiver's Private Key] ---> |Decryption |
     | Algorithm |      (Freely available to all) |   (Kept strictly secret)        | Algorithm |
     +-----+-----+                                |                                 +-----+-----+
           |                                      |                                       ^
           v                                      |                                       |
     +-----+-----+                                |                                       |
     |Ciphertext | ================= (Insecure Network) ==================================+
     +-----------+
```

---

### 5. Strategic Advantages of Public Key Cryptography

*(From PDF Page 18)*

1. **Solves Key Distribution:** Sender and receiver never need to share or transmit a secret key over the network.
2. **Enhanced Security:** Even if an attacker intercepts the public key and the encrypted ciphertext, they cannot decrypt the message without the private key.
3. **Supports User Authentication & Digital Signatures:** Enables senders to sign documents mathematically so that recipients can verify the sender's identity.
4. **Non-Repudiation:** A sender cannot deny having sent a message if it was encrypted or signed using their unique private key.

---

### 6. Key Differences: Symmetric vs. Asymmetric Cryptography

*(High-probability 4-mark exam comparison table)*

| Feature / Dimension | Symmetric Key Cryptography | Asymmetric Key Cryptography (Public Key) |
| --- | --- | --- |
| **Number of Keys** | 1 single key shared between sender and receiver. | 2 mathematically linked keys (Public and Private). |
| **Key Distribution** | Difficult and insecure (key must be shared prior to communication). | Simple and secure (Public key is shared openly; private key is never transmitted). |
| **Encryption / Decryption** | Same key performs both encryption and decryption. | Public key encrypts, Private key decrypts. |
| **Speed & Performance** | Very fast (used for bulk data encryption). | Slower due to complex mathematical computations. |
| **Algorithms** | DES, AES, Triple-DES, Blowfish. | RSA, Diffie-Hellman, ECC. |

---

## Module 5: The RSA Algorithm & Solved Numericals

### 2. Ideal Theory Answer: Overview and Steps of the RSA Algorithm

*(From PDF Page 20 & 21)*

#### A. Overview

- The **RSA algorithm** is an asymmetric (public key) encryption technique invented by **Ron Rivest, Adi Shamir, and Leonard Adleman**.
- It provides strong security because its mathematical strength relies on the extreme difficulty of **factoring very large prime numbers**.

---

#### B. Standard Exam Flow Diagram

```text
                                   [ Calculation of e, d, n ]
                                           /         \
                                        /               \
                       Public Key (e, n)                Private Key (d, n)
                               |                                 |
                               v                                 v
      +----------+     +---------------+                 +---------------+     +----------+
      | Sender A | --> |   Encryption  | ==============> |   Decryption  | --> |Receiver B|
      +----------+     | C = M^e mod n |   Ciphertext    | M = C^d mod n |     +----------+
           |           +---------------+                 +---------------+          ^
           v                                                                        |
     Plaintext (M)                                                            Plaintext (M)
```

---

#### C. The 6 Algorithmic Steps

*(Memorize these 6 exact mathematical formulas)*

1. **Step 1: Select Two Large Prime Numbers**
    - Choose two distinct prime numbers, $p$ and $q$.
2. **Step 2: Calculate the Modulus** $n$

    $$n = p \times q$$

    - $n$ is the common modulus used in both the public key and the private key.
3. **Step 3: Calculate Euler's Totient Function** $\phi(n)$

    $$\phi(n) = (p - 1) \times (q - 1)$$

4. **Step 4: Select Public Key Exponent** $e$
    - Choose an integer $e$ such that:

    $$1 < e < \phi(n) \quad \text{and} \quad \gcd(e, \phi(n)) = 1$$

    - *(That is, $e$ and $\phi(n)$ must be coprime / have no common factors)*.
    - **Public Key =** $(e, n)$.
5. **Step 5: Calculate Private Key Exponent** $d$
    - Calculate $d$ such that:

    $$d \times e \equiv 1 \pmod{\phi(n)} \quad \text{or} \quad d = \frac{1 + (k \times \phi(n))}{e}$$

    - *(where $k$ is an integer chosen such that $d$ comes out as a whole number)*.
    - **Private Key =** $(d, n)$.
6. **Step 6: Encryption and Decryption Formulas**
    - **Encryption (Sender):**

    $$C = M^e \bmod n$$

    - **Decryption (Receiver):**

    $$M = C^d \bmod n$$

---

### 3. Solved Numerical Example (Directly from PDF Page 22)

*(Practice this complete step-by-step calculation to score full 7 marks)*

**Problem:** Perform RSA key generation, encryption, and decryption using prime numbers $p = 5$ and $q = 11$, with public exponent $e = 7$ and message $M = 15$.

---

**Step 1: Given Primes**

$$p = 5, \quad q = 11$$

**Step 2: Calculate** $n$

$$n = p \times q = 5 \times 11 = \mathbf{55}$$

**Step 3: Calculate** $\phi(n)$

$$\phi(n) = (p - 1) \times (q - 1) = (5 - 1) \times (11 - 1) = 4 \times 10 = \mathbf{40}$$

**Step 4: Establish Public Key** $(e, n)$

- Given $e = 7$.
- Check condition: $1 < 7 < 40$ and $\gcd(7, 40) = 1$ (Coprime: valid!).
- **Public Key =** $(7, 55)$.

**Step 5: Encryption (Finding Ciphertext** $C$**)**

Given message $M = 15$:

$$C = M^e \bmod n = 15^7 \bmod 55$$

- $15^7 = 170{,}859{,}375$
- $170{,}859{,}375 \div 55 = 3{,}106{,}534$ remainder $5$
- **Ciphertext** $C = 5$.

**Step 6: Calculate Private Key** $(d, n)$

$$d = \frac{1 + (k \times \phi(n))}{e} = \frac{1 + (k \times 40)}{7}$$

- Test integer values for $k$:
    - If $k = 1 \implies \frac{1 + 40}{7} = \frac{41}{7}$ (not an integer)
    - If $k = 2 \implies \frac{1 + 80}{7} = \frac{81}{7}$ (not an integer)
    - If $k = 3 \implies \frac{1 + 120}{7} = \frac{121}{7}$ (not an integer)
    - If $k = 4 \implies \frac{1 + 160}{7} = \frac{161}{7} = \mathbf{23}$
- **Private Key =** $(23, 55)$.

**Step 7: Decryption (Recovering Plaintext** $M$**)**

Using $C = 5$ and private key $d = 23$:

$$M = C^d \bmod n = 5^{23} \bmod 55$$

- Using modular properties:
    - $5^1 \bmod 55 = 5$
    - $5^2 \bmod 55 = 25$
    - $5^3 \bmod 55 = 125 \bmod 55 = 15$
    - $5^6 \bmod 55 = (15 \times 15) \bmod 55 = 225 \bmod 55 = 5$
    - Since $5^6 \equiv 5 \pmod{55}$, we have $5^{23} = (5^6)^3 \times 5^5$:

    $$5^{23} \bmod 55 = (5^3 \times 45) \bmod 55 = (15 \times 45) \bmod 55 = 675 \bmod 55 = \mathbf{15}$$

- **Recovered Plaintext** $M = 15$ (Matches original message!).

---

## Module 6: User Authentication & Cryptographic Hash Functions

### 1. Part A: User Authentication Architecture

#### A. Definition

- **Authentication** is the process of verifying the identity of a user or information.
- **User authentication** is specifically the process of verifying the identity of a user when that user logs into a computer system.

---

#### B. The 3 Types of Authentication Systems

**1. Single-Factor Authentication:**

- This was the **first method of security** that was developed.
- The user enters a **username and password** to confirm their identity.
- If the credentials match system records, access is permitted; if either is wrong, the user is not allowed to log in or access the system.

**2. Two-Factor Authentication (2FA):**

- In this system, the user provides a **username and password** plus **additional information/factors**.
- Examples of secondary factors used to secure the system:
    - **Wireless token**
    - **Virtual token**
    - **OTP (One-Time Password)**

**3. Multi-Factor Authentication (MFA):**

- Requires **more than one factor of authentication**.
- **Security Benefits:**
    - Provides superior security to the user.
    - Any type of **keylogger** or **phishing attack** is prevented because the attacker cannot capture all required factors.
    - Assures that information will not be stolen.

---

### 2. Part B: Cryptographic Hash Functions

#### A. Definition & Mathematical Concept

- A **hash function** is a mathematical algorithm that takes an arbitrary/variable number of characters (**Message $M$ of arbitrary length**) and converts it into a string with a **fixed number of characters** called a **Hash Value** or **Message Digest** (typically written in hexadecimal notation).

```text
                 Message M (Arbitrary Length)
                             |
                             v
                  +---------------------+
                  |    Hash Function    |
                  +----------+----------+
                             |
                             v
                  Hash Value h (Fixed Length)
```

*(Reference Diagram: `06_SIXTH-CHAPTER.pdf`, Page 25)*

- Converts a given numeric or alphanumeric key into a small practical integer value.

---

#### B. Design of Hashing Algorithms & Iterative Block Processing

- Hashing operates on **two fixed-size blocks of data** to create a hash code:
    - The size of each data block typically ranges from **128 bits to 512 bits** depending on the specific algorithm.
- **Rounds like a Block Cipher:**
    - The algorithm involves rounds of the hash function.
    - Each round takes a fixed-size input consisting of a **combination of the most recent message block** and the **output of the previous round** (initialized by a **seed value**).
    - This process is repeated for as many rounds as required to process the entire message.

```text
                    Message Block 1                  Message Block n
                           |                                |
                           v                                v
  [Seed Value] ---> [Hash Function] ------> ... ---> [Hash Function] ---> [Hash Value]
```

---

#### C. The Avalanche Effect

- Because the hash value of the first message block becomes an input to the second hash operation, its output alters the result of the third operation, cascading across the entire chain.
- **Definition:** A minute change in the original input message (even a single bit change) results in a completely transformed, unpredictable final hash value. This property is known as the **Avalanche Effect of hashing**.

---

#### D. Secure Hash Algorithm (SHA) & Implementation

- **SHA** is a modified version of **MD5** used for hashing data and digital certificates.
- It shortens input data into a smaller form that cannot be easily reversed, using **bitwise operations, modular additions, and compression functions**.
- **Family of Algorithms:**
    - `MD2`, `MD5`, `SHA-1`, `SHA-224`, `SHA-256`, `SHA-384`, `SHA-512`.
- **Implementation Flow in Java (`java.security` package):**
    1. Algorithms are initialized using the static factory method: `MessageDigest.getInstance("SHA-256")`.
    2. The digest value is calculated from the input text and returned as a **byte array**.
    3. The `BigInteger` class converts the byte array into signum representation.
    4. The result is converted into a **hexadecimal format** to produce the final `MessageDigest`.

---

## Module 7: Digital Signatures

### A. Overview & Key Objectives

- A **Digital Signature** is a mathematical technique used to validate the **authenticity and integrity** of a digital message, document, or software.
- It is the digital equivalent of an ink-on-paper handwritten signature or stamped seal, but it offers far greater cryptographic security.
- **Core Security Objectives Provided by Digital Signatures:**
    1. **Authentication:** Validates the identity of the sender beyond doubt.
    2. **Data Integrity:** Guarantees that the message was not modified, tampered with, or corrupted in transit.
    3. **Non-Repudiation:** Ensures the sender cannot deny having sent the message, because only the sender has access to their own unique private signing key.

### B. The Two Keys Used in Digital Signatures (Crucial Concept!)

Notice how the keys are used here compared to standard encryption:

- **Sender's Private Key:** Used at the **Sender side to SIGN / create the digital signature** (kept strictly secret by the sender).
- **Sender's Public Key:** Used at the **Receiver side to VERIFY the signature** (available openly to everyone).

### C. Standard Examination Flow Diagram (Sender & Receiver Sides)

```text
============================== SENDER SIDE ==============================

                     +----------------------+
                     |   Original Message   |
                     +----------+-----------+
                                |
                     +----------v-----------+
                     |    Hash Function     |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     |    Message Digest    | (Hash Value)
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     | Encryption Algorithm | <--- [Sender's Private Key]
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     |  DIGITAL SIGNATURE   |
                     +----------+-----------+
                                |
          +---------------------+---------------------+
          |                                           |
          v                                           v
  [Original Message]                         [Digital Signature]
          \                                           /
           \================ (Network) ===============/


============================= RECEIVER SIDE =============================

    Incoming Data: [Original Message]   +   [Digital Signature]
                          |                         |
                          v                         v
               +--------------------+    +--------------------+
               |   Hash Function    |    |Decryption Algorithm| <-- [Sender's Public Key]
               +----------+---------+    +----------+---------+
                          |                         |
                          v                         v
                  Message Digest 1          Message Digest 2
                          |                         |
                          +------------+------------+
                                       |
                                       v
                                /-------------\
                               <  Are Digests  >
                               <    EQUAL?     >
                                \-------------/
                                  /         \
                            YES  /           \  NO
                                v             v
                         [ Signature     [ Signature INVALID!
                           VERIFIED /      Data tampered with
                           ACCEPTED ]      or fake sender ]
```

### D. Step-by-Step Operations

#### 1. Sender-Side Operations (Signing the Message)

1. **Generate Message Digest:** The sender takes the original message and passes it through a cryptographic **Hash Function** (e.g., SHA-256) to produce a fixed-length **Message Digest**.
2. **Encrypt the Digest (Create Signature):** The sender encrypts this message digest using their own **Private Key**. The resulting encrypted output is the **Digital Signature**.
3. **Transmit:** The sender bundles the **Original Message together with the Digital Signature** and transmits them across the network to the receiver.

#### 2. Receiver-Side Operations (Verifying the Signature)

1. **Decrypt Signature:** The receiver takes the incoming digital signature and decrypts it using the **Sender's Public Key**. This produces **Message Digest 2** (the hash computed by the sender).
2. **Re-compute Hash:** The receiver independently runs the received original message through the **exact same Hash Function** to calculate **Message Digest 1**.
3. **Compare & Verify:**
    - If **Message Digest 1 == Message Digest 2**: The signature is **valid and verified**! This proves:
        - The message was genuinely created by the claimed sender (Authentication & Non-repudiation).
        - The message content was not altered in transit (Integrity).
    - If the digests **do not match**: The receiver rejects the message because it was either modified in transit or sent by an unauthorized party.

---

## Module 8: Firewalls & Network Perimeter Security

### A. Definition & Working Principle of a Firewall

- **Definition:** A **firewall** is a network security system that **monitors and controls incoming and outgoing network traffic** based on **predetermined security rules**.
- **Implementation:** A firewall can be **hardware, software, or a combination of both**. It inspects network traffic passing through it and either **accepts or rejects** the messages based on a set of rules (security protocols).
- **Perimeter Barrier:**
    - **All traffic must pass through the firewall**, whether moving from inside to outside or vice versa.
    - **Only authorized traffic** that matches the local security policy is permitted to cross the barrier.

### B. Standard Exam Architecture Diagram 1: Perimeter Firewall

```text
               [ Internal Network ]
                        |
                        | (Data)
                        v
               +------------------+
               |  # # # # # # # # |
               |  #  FIREWALL   # |  <--- (Filters traffic based on rules)
               |  # # # # # # # # |
               +------------------+
                        |
                        | (Data)
                        v
               [ External Network ]
```

### C. Types of Firewalls (Detailed Analysis)

#### 1. Packet Filtering Firewall

- **How it works:** A packet filtering firewall applies a **set of rules to each incoming or outgoing IP packet** and then decides either to **forward or discard** the packet.
- **Rule Decision Logic:** If a packet matches an established rule, the corresponding action is executed; otherwise, a **default action** (typically *discard* or *forward*) is taken.
- **Header Fields Inspected by the Rules:**
    1. **Source IP Address:** The IP address of the system that generated the IP packet.
    2. **Destination IP Address:** The IP address of the system the packet is trying to reach.
    3. **Interface:** The specific router interface (port) from which the packet arrived or where it is destined.
    4. **Protocols:** The transport protocol used (e.g., TCP, UDP, ICMP).
    5. **Ports:** Source and destination application port numbers (e.g., Port 80 for HTTP, Port 21 for FTP).

```text
                     +----------------------------+
                     |  Packet Filtering Firewall |
                     +--------------+-------------+
                                    |
            +-----------------------+-----------------------+
            |                       |                       |
            v                       v                       v
     [ Match Rule? ]        [ Forward Packet ]      [ Discard Packet ]
```

#### 2. Application-Level Gateway (Proxy Server)

- **How it works:** An **application-level gateway** is also known as a **proxy server** because it acts as an intermediary (a proxy) and controls the flow of **application-level traffic**.
- **Connection Architecture:**
    - It acts as a middleman between the internal user and the outside host.
    - An internal user contacts the application gateway using a TCP/IP application such as **TELNET, FTP, SMTP, or HTTP**.
- **Step-by-Step Operation:**
    1. The application gateway prompts the user/host for the **remote destination host** with which they want to communicate.
    2. The user must provide valid identification and **authentication information** (e.g., user ID and password).
    3. Once authenticated, the gateway contacts the target application on the remote host and **relays TCP segments containing application data** back and forth between the two endpoints.

```text
 [ Inside Host ] <=== (Inside Connection) ===> [ Application-Level Gateway ] <=== (Outside Connection) ===> [ Outside Host ]
                                               |       (Proxy Server)      |
                                               | - TELNET                  |
                                               | - FTP                     |
                                               | - SMTP                    |
                                               | - HTTP                    |
                                               +---------------------------+
```

*(Reference Diagram: `06_SIXTH-CHAPTER.pdf`, Page 37)*

#### 3. Other Firewall Types Mentioned in Syllabus Notes

*(List these on your exam sheet from Page 35 of `06_SIXTH-CHAPTER.pdf` to show complete coverage)*

- **Circuit-Level Gateway**
- **Stateful Packet Filtering**
- **Next-Generation Firewall (NGFW)**

---

## Editor's Verification Notes

These items were left as in your teacher's notes or changed only in layout. Please confirm them against the original reference notes.

1. **[DIAGRAM NEEDS VERIFICATION] Single DES round (Module 2, D):** The pasted diagram had lost all line breaks. I reconstructed it from the components named in the paste and the Mixer/Swapper text, and kept the original pasted text below it. Exact positions of the lines (including how the key and the swap are drawn) may differ from your teacher's diagram.
2. **Digital Signature sender diagram:** The pasted network line ended in two slashes (`//`). I used a single `/` so it lines up with the arrow above it.
3. **"Factoring very large prime numbers" (Module 4, section 3 and Module 5, A):** Strictly, RSA's security rests on the difficulty of factoring a very large *composite* number (the product of two primes). Kept as written.
4. **"SHA is a modified version of MD5" (Module 6, D):** Kept as written; please confirm this is how your teacher states it.
5. **Numbering corrections:**
    - Module 1, Topic B: the Block vs. Stream table was numbered "3." again; it is now "4.".
    - Module 2, C: the pasted list repeated the heading "Step-by-Step Explanation" as item 1 and numbered the next two items "2." and "2."; I removed the duplicate and renumbered 1 to 5.
    - Module 5 begins at "2." (no section 1 in the pasted text), followed by "3.".
6. **Removed copy-paste artifacts:** citation markers such as `[1][2]`, stray digit suffixes (e.g. `34`, `112`, `313`), the text `more_horiz`, HTML entities (`&lt;`, `&gt;`, `&amp;`), doubled backslashes (`\\`) in diagrams and formulas, stray `\*` and `\_` escapes, and the broken table fragments in the symmetric-key and hash diagrams.
7. **Diagram realignment:** The symmetric-key, high-level DES, asymmetric-key, RSA and iterative-hash diagrams had shifted labels, boxes or connector characters, so I realigned them without changing any labels or arrow directions. In the RSA diagram I widened the Encryption and Decryption boxes so the formulas fit inside them.
