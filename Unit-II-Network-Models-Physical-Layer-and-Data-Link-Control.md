# Unit II – Network Models, Physical Layer and Data Link Control

## Table of Contents
1. [Introduction & Unit Overview](#1-introduction--unit-overview)
2. [Topic 1: The OSI Reference Model](#2-topic-1-the-osi-reference-model)
   - [2.1 Background and Concept of Open Systems](#21-background-and-concept-of-open-systems)
   - [2.2 The 7-Layer OSI Architecture](#22-the-7-layer-osi-architecture)
   - [2.3 Peer-to-Peer Processes](#23-peer-to-peer-processes)
   - [2.4 Layer Interfaces and Modularity](#24-layer-interfaces-and-modularity)
   - [2.5 Data Encapsulation and Decapsulation](#25-data-encapsulation-and-decapsulation)
3. [Topic 2: Physical Layer Fundamentals](#3-topic-2-physical-layer-fundamentals)
   - [3.1 Definition and Physical Layer Role](#31-definition-and-physical-layer-role)
   - [3.2 Data and Signals: Analog vs Digital](#32-data-and-signals-analog-vs-digital)
   - [3.3 Key Signal Metrics: Bit Rate and Bit Length](#33-key-signal-metrics-bit-rate-and-bit-length)
   - [3.4 Transmission Impairments: Attenuation, Distortion, and Noise](#34-transmission-impairments-attenuation-distortion-and-noise)
   - [3.5 Signal-to-Noise Ratio (SNR) and Decibel (dB) Calculations](#35-signal-to-noise-ratio-snr-and-decibel-db-calculations)
   - [3.6 Data Rate Limits: Nyquist Bit Rate and Shannon Capacity](#36-data-rate-limits-nyquist-bit-rate-and-shannon-capacity)
   - [3.7 Functions of the Physical Layer](#37-functions-of-the-physical-layer)
4. [Topic 3: Data Link Control (DLC) & Framing](#4-topic-3-data-link-control-dlc--framing)
   - [4.1 Concept and Purpose of Data Link Control](#41-concept-and-purpose-of-data-link-control)
   - [4.2 Framing Techniques: Fixed-Size vs Variable-Size Framing](#42-framing-techniques-fixed-size-vs-variable-size-framing)
   - [4.3 Character-Oriented Protocols and Byte Stuffing](#43-character-oriented-protocols-and-byte-stuffing)
   - [4.4 Bit-Oriented Protocols and Bit Stuffing](#44-bit-oriented-protocols-and-bit-stuffing)
5. [Topic 4: Data Link Layer Services](#5-topic-4-data-link-layer-services)
   - [4.1 Unacknowledged Connectionless Service](#51-unacknowledged-connectionless-service)
   - [4.2 Acknowledged Connectionless Service](#52-acknowledged-connectionless-service)
   - [4.3 Acknowledged Connection-Oriented Service](#53-acknowledged-connection-oriented-service)
6. [Topic 5: Data Link Layer Protocols](#6-topic-5-data-link-layer-protocols)
   - [5.1 Protocol Classification and Taxonomy](#61-protocol-classification-and-taxonomy)
   - [5.2 Protocols for Noiseless Channels](#62-protocols-for-noiseless-channels)
     - [5.2.1 Simplest Protocol](#521-simplest-protocol)
     - [5.2.2 Stop-and-Wait Protocol](#522-stop-and-wait-protocol)
   - [5.3 Protocols for Noisy Channels (Automatic Repeat Request - ARQ)](#63-protocols-for-noisy-channels-automatic-repeat-request---arq)
     - [5.3.1 Stop-and-Wait ARQ](#531-stop-and-wait-arq)
     - [5.3.2 Go-Back-N ARQ](#532-go-back-n-arq)
     - [5.3.3 Selective Repeat ARQ](#533-selective-repeat-arq)
   - [5.4 Piggybacking Technique](#64-piggybacking-technique)
7. [Numerical Concepts & Step-by-Step Solved Examples](#7-numerical-concepts--step-by-step-solved-examples)
8. [Important Definitions & Terminology Summary](#8-important-definitions--terminology-summary)
9. [Comprehensive Comparison Tables](#9-comprehensive-comparison-tables)
10. [Quick-Reference List of PDF Diagrams](#10-quick-reference-list-of-pdf-diagrams)
11. [Chapter Summary & Exam Recap](#11-chapter-summary--exam-recap)

---

## 1. Introduction & Unit Overview

Computer networking relies on structured architecture to enable reliable communication between diverse computing systems. Unit II covers two fundamental aspects of the Open Systems Interconnection (OSI) framework:
1. **Network Models**: The 7-layer OSI reference model, layered architecture, interfaces, peer-to-peer processes, and data encapsulation mechanisms.
2. **Physical Layer & Data Link Control**: Physical transmission characteristics, signal impairments, data rate theoretical limits (Nyquist and Shannon capacity), framing techniques (byte and bit stuffing), DLC service categories, and link control protocols for noiseless and noisy channels.

---

## 2. Topic 1: The OSI Reference Model

### 2.1 Background and Concept of Open Systems
The **Open Systems Interconnection (OSI) model** is a layered framework developed by the **International Standards Organization (ISO)** in **1947**. The ISO is a multinational body dedicated to establishing worldwide standards for international trade, engineering, and technology.

> **Key Definition:** An **Open System** is a set of protocols that allows any two different computing systems to communicate with each other regardless of their underlying hardware and software architectures.

The core objective of the OSI reference model is to facilitate inter-system communication without requiring changes to the internal hardware logic or software implementations of the communicating machines.

### 2.2 The 7-Layer OSI Architecture
The OSI model divides the communication process into **seven ordered, separate but related layers**. Each layer defines a specific portion of the overall network communication process.

**[📌 REFER TO PDF DIAGRAM: Seven Layers of the OSI Model – PDF PAGE NO. 01]**

The 7 layers are organized into two functional categories:
- **Upper Layers (Layers 5, 6, 7)**: Application, Presentation, and Session layers. These layers are oriented towards software applications and end-user services.
- **Lower Layers (Layers 1, 2, 3, 4)**: Physical, Data Link, Network, and Transport layers. These layers manage the end-to-end data flow through the physical network infrastructure.

#### Summary of the 7 Layers (Bottom-to-Top):
1. **Layer 1 – Physical Layer**: Responsible for bit-level transmission across physical media.
2. **Layer 2 – Data Link Layer**: Manages node-to-node frame delivery, error control, and flow control.
3. **Layer 3 – Network Layer**: Handles packet routing and logical addressing across multiple networks.
4. **Layer 4 – Transport Layer**: Ensures end-to-end process-to-process message delivery and error recovery.
5. **Layer 5 – Session Layer**: Establishes, maintains, and synchronizes interaction sessions between systems.
6. **Layer 6 – Presentation Layer**: Manages syntax, data formatting, encryption, and compression.
7. **Layer 7 – Application Layer**: Provides network services directly to end-user application software.

### 2.3 Peer-to-Peer Processes
In a network environment, communication occurs between corresponding layers on different devices through **peer-to-peer processes**.

**[📌 REFER TO PDF DIAGRAM: Devices and Intermediate Nodes in OSI Communication – PDF PAGE NO. 03]**

- When Device A sends data to Device B, the message travels down through the layers of Device A, passes across the physical transmission medium, and ascends through the layers of Device B.
- **Intermediate Nodes**: Routers and switches located between Device A and Device B process data using only the **lower three layers** (Physical, Data Link, and Network layers) of the OSI model.
- At the **Physical Layer**, communication is direct; Device A sends a stream of raw binary bits directly across the physical medium to Device B.
- At higher layers, communication is virtual or logical: each layer on the sending machine adds control information and passes the package down to the layer below it, while the corresponding layer on the receiving machine interprets that control information.

### 2.4 Layer Interfaces and Modularity
To ensure seamless communication, an **interface** is established between each pair of adjacent layers.

**[📌 REFER TO PDF DIAGRAM: The Interface Between Layers in the OSI Model – PDF PAGE NO. 05]**

- **Interface Definition**: An interface defines the specific operations and services that a lower layer offers to the upper adjacent layer.
- **Passing Data**: Passing data and network information across adjacent layers is enabled by well-defined interfaces.
- **Layered Modularity**: Well-defined interfaces provide **modularity** to the network architecture. As long as the interface between two layers remains unchanged, the internal implementation of a layer can be modified or upgraded without affecting adjacent layers.

### 2.5 Data Encapsulation and Decapsulation
Data transmission in the OSI model involves **data encapsulation** at the sending host and **decapsulation** at the receiving host.

**[📌 REFER TO PDF DIAGRAM: Data Encapsulation and Decapsulation Process – PDF PAGE NO. 07]**

#### Working Principle of Encapsulation:
1. The user message originates at Layer 7 (**Application Layer**) as data unit $D_7$.
2. As the message moves down through the layers at the source machine, each layer adds control information in the form of a **header** ($H_7, H_6, H_5, H_4, H_3, H_2$).
3. At Layer 2 (**Data Link Layer**), both a header ($H_2$) and a **trailer** ($T_2$) are added to the data unit. The trailer carries error-detection redundancy bits.
4. The headers and trailer form a protective envelope surrounding the data received from the layer above.
5. When the formatted data unit reaches Layer 1 (**Physical Layer**), it is converted into an electromagnetic or optical signal stream and transmitted across the physical link.

#### Working Principle of Decapsulation:
1. At the receiving machine, the incoming signals are converted back into binary data by the Physical Layer.
2. The message is unwrapped layer-by-layer in ascending order.
3. Each layer receives the data unit, processes and strips off the control header (and trailer at Layer 2) intended for it, and passes the remaining payload up to the adjacent higher layer.

---

## 3. Topic 2: Physical Layer Fundamentals

### 3.1 Definition and Physical Layer Role
The **Physical Layer** is the first (base) layer of the OSI reference model. It is the only layer that handles the actual physical connectivity between two distinct communicating stations.

- **Scope**: Defines hardware equipment, cabling, physical wiring, signal frequencies, pin configurations, and voltage pulses representing binary signals.
- **Service Provision**: Provides physical transmission services directly to the Data Link Layer (Layer 2).

### 3.2 Data and Signals: Analog vs Digital
Data must be converted into **electromagnetic signals** before transmission across physical media.

#### Data Categories:
- **Analog Data**: Refers to continuous information that takes continuous values over a given range (e.g., human voice, temperature).
- **Digital Data**: Refers to discrete information that takes discrete states (e.g., binary 0s and 1s stored in computer memory).

#### Signal Categories:
- **Analog Signals**: Continuous electromagnetic wave forms that possess infinitely many intensity levels over a time period.
- **Digital Signals**: Discrete square wave voltage pulses that possess only a limited number of defined values (typically binary positive voltage for 1 and zero voltage for 0).

**[📌 REFER TO PDF DIAGRAM: Analog Signal vs Digital Signal Waveforms – PDF PAGE NO. 10]**

### 3.3 Key Signal Metrics: Bit Rate and Bit Length

- **Bit Rate**: The number of bits transmitted in one second, expressed in **bits per second (bps)**.
- **Bit Duration**: The time required to transmit a single bit ($\text{Bit Duration} = \frac{1}{\text{Bit Rate}}$).
- **Bit Length**: The physical distance that a single bit occupies on the transmission medium.

$$\text{Bit Length} = \text{Propagation Speed} \times \text{Bit Duration}$$

### 3.4 Transmission Impairments: Attenuation, Distortion, and Noise
Signals traveling through real-world transmission media experience **imperfections** and **signal impairments**, meaning the signal received at the destination differs from the signal transmitted at the source.

**[📌 REFER TO PDF DIAGRAM: Causes of Transmission Impairment – PDF PAGE NO. 12]**

#### 1. Attenuation (Loss of Energy)
- **Concept**: As a signal travels through a medium, it loses energy due to the electrical resistance of the medium.
- **Effect**: Signal amplitude decreases over distance.
- **Solution**: **Amplifiers** are placed along the transmission path to boost the attenuated signal strength.

**[📌 REFER TO PDF DIAGRAM: Attenuation and Amplification of Signals – PDF PAGE NO. 12]**

#### 2. Distortion (Change in Signal Shape)
- **Concept**: Occurs in **composite signals** composed of multiple frequency components.
- **Cause**: Each frequency component travels at its own propagation speed through the medium, arriving at the destination with different phase shifts and delays.
- **Effect**: The composite signal changes its overall form or shape.

**[📌 REFER TO PDF DIAGRAM: Distortion in Composite Signals – PDF PAGE NO. 13]**

#### 3. Noise (Unwanted Random Disturbances)
**Noise** represents random disturbances or fluctuations added to an analog or digital signal that distort the underlying information.

Noise is classified into four categories:
1. **Thermal Noise**: Random motion of electrons in a conductor, creating an extra signal not sent by the transmitter.
2. **Intermodulation Induced Noise**: Extra signals created when signals of different frequencies share the same medium and interact non-linearly.
3. **Crosstalk**: The electromagnetic effect of one wire inducing unwanted signals in an adjacent wire (one wire acts as a transmitting antenna, the other as a receiving antenna).
4. **Impulse Noise**: Sudden, high-energy spikes of very short duration caused by power lines, heavy machinery, or lightning strikes.

### 3.5 Signal-to-Noise Ratio (SNR) and Decibel (dB) Calculations

#### Decibel ($dB$) Unit
The **decibel ($dB$)** measures the relative strength of two signals or the signal power ratio between two points. It indicates power loss (attenuation) or power gain (amplification).

$$dB = 10 \log_{10} \left( \frac{P_2}{P_1} \right)$$

- $P_1$ = Power of signal at Point 1 (input power)
- $P_2$ = Power of signal at Point 2 (output power)
- **Negative $dB$ value**: Indicates signal **attenuation** (loss).
- **Positive $dB$ value**: Indicates signal **amplification** (gain).

#### Signal-to-Noise Ratio ($SNR$)
The **Signal-to-Noise Ratio ($SNR$)** measures signal quality by comparing average signal power to average noise power:

$$SNR = \frac{\text{Average Signal Power}}{\text{Average Noise Power}}$$

- High $SNR$ means high signal quality (wanted signal dominates unwanted noise).
- $SNR$ is often expressed in decibels: $SNR_{dB} = 10 \log_{10}(SNR)$.

### 3.6 Data Rate Limits: Nyquist Bit Rate and Shannon Capacity
The maximum rate at which data can be sent over a transmission channel depends on three critical factors:
1. The **available bandwidth** of the channel.
2. The **number of signal levels** ($L$) used.
3. The **quality of the channel** (level of noise present).

#### 1. Nyquist Bit Rate (For Noiseless Channels)
Formulated by Harry Nyquist for an ideal **noiseless channel**:

$$\text{Bit Rate} = 2 \times \text{Bandwidth} \times \log_2(L)$$

- $\text{Bandwidth}$: Channel bandwidth in Hertz ($Hz$).
- $L$: Number of signal levels used to represent data.
- $\text{Bit Rate}$: Theoretical maximum data rate in bits per second ($bps$).

#### 2. Shannon Channel Capacity (For Noisy Channels)
Formulated by Claude Shannon in **1944** to determine the theoretical highest data rate for a **noisy channel**:

$$\text{Capacity} = \text{Bandwidth} \times \log_2(1 + SNR)$$

- $\text{Capacity}$: Maximum channel capacity in bits per second ($bps$).
- $\text{Bandwidth}$: Channel bandwidth in Hertz ($Hz$).
- $SNR$: Signal-to-Noise Ratio (linear scale, not $dB$).

### 3.7 Functions of the Physical Layer
1. **Establishing Physical Connections**: Configures and activates physical media connections between nodes.
2. **Sequential Data Delivery**: Transmits raw binary streams sequentially bit-by-bit.
3. **Terminating Physical Connections**: Deactivates links upon communication completion.
4. **Frame-to-Bit Conversion**: Converts Data Link Layer frames into electrical/optical pulses suitable for the physical transmission medium.

**[📌 REFER TO PDF DIAGRAM: Physical Layer Function and Bit Transmission – PDF PAGE NO. 17]**

---

## 4. Topic 3: Data Link Control (DLC) & Framing

### 4.1 Concept and Purpose of Data Link Control
**Data Link Control (DLC)** encompasses the design specifications and operating procedures required for communication between two adjacent nodes (**node-to-node communication**).

Functions of DLC include:
- **Framing**: Message packetization.
- **Flow Control**: Preventing fast senders from overwhelming slow receivers.
- **Error Control**: Detecting and correcting bit errors during transmission.

### 4.2 Framing Techniques: Fixed-Size vs Variable-Size Framing
The Data Link Layer encapsulates Network Layer packets into **frames**. Framing separates messages from different sources and destinations by prepending sender and destination addresses.

- **Single-Bit Error Protection**: Dividing large messages into smaller individual frames ensures that a single-bit error damages only one small frame rather than destroying the entire transmission.

#### 1. Fixed-Size Framing
- Frame boundaries do not require explicit flag markers; the constant frame length acts as the implicit delimiter.
- **Practical Example**: **Asynchronous Transfer Mode (ATM)** Wide Area Networks, which utilize fixed-size 53-byte frames called **cells**.

#### 2. Variable-Size Framing
- Frames vary in length, requiring explicit delimiter patterns to mark the beginning and end of each frame.
- Two distinct framing implementations are used:
  1. **Character-Oriented (Byte-Oriented) Protocols**
  2. **Bit-Oriented Protocols**

### 4.3 Character-Oriented Protocols and Byte Stuffing
In **Character-Oriented Protocols**, data is transmitted as 8-bit characters (bytes).

- **Frame Structure**: Composed of a Start Flag byte, Header, Variable-length data characters, Trailer (carrying error detection redundancy bits), and an End Flag byte.

**[📌 REFER TO PDF DIAGRAM: A Frame in a Character-Oriented Protocol & Byte Stuffing – PDF PAGE NO. 23]**

#### The Data Translucency Problem and Byte Stuffing:
- If the binary data payload contains a byte that matches the 8-bit Flag pattern, the receiver misinterprets it as the end of the frame.
- **Solution — Byte Stuffing (Character Stuffing)**: Whenever a Flag byte or an Escape byte (`ESC`) occurs naturally inside the user data payload, the sender inserts an extra special byte called the **Escape character (`ESC`)** immediately before it.
- **Receiver Operation**: The receiving station detects the `ESC` byte, removes (unstuffs) it from the data stream, and interprets the following byte purely as data rather than a framing control signal.

### 4.4 Bit-Oriented Protocols and Bit Stuffing
In **Bit-Oriented Protocols**, the data payload is treated strictly as a continuous sequence of binary bits (representing text, images, audio, or video).

- **Delimiter Flag**: A special 8-bit pattern `01111110` (six consecutive 1s bounded by 0s) is used as the framing flag.

**[📌 REFER TO PDF DIAGRAM: A Frame in a Bit-Oriented Protocol & Bit Stuffing Mechanism – PDF PAGE NO. 24]**

#### Bit Stuffing Rule:
- To prevent the receiver from misinterpreting a `01111110` pattern inside the user data payload as a Flag, a bit-stuffing mechanism is enforced.
- **Sender Procedure**: Whenever the sender encounters **five consecutive 1s** in the data payload, it automatically stuff/inserts an extra **`0` bit** immediately after the fifth 1, regardless of the value of the next bit.
- **Receiver Procedure**: When the receiver detects five consecutive 1s followed by a `0` bit, it automatically removes (unstuffs) the `0` bit to restore the original data stream.

**[📌 REFER TO PDF DIAGRAM: Bit Stuffing and Unstuffing Data Stream Example – PDF PAGE NO. 25]**

---

## 5. Topic 4: Data Link Layer Services

The Data Link Layer generally provides three categories of services to the Network Layer:

### 5.1 Unacknowledged Connectionless Service
- **Mechanism**: The source machine transmits independent data frames to the destination machine without requiring frame acknowledgements.
- **Connection**: No logical connection is established prior to or released after transmission.
- **Application**: Useful over highly reliable channels (e.g., optical fiber links) or real-time traffic where speed is prioritized over error recovery.

### 5.2 Acknowledged Connectionless Service
- **Mechanism**: Each transmitted frame is individually acknowledged by the receiver using a Stop-and-Wait confirmation mechanism.
- **Error Handling**: If an acknowledgement is not received within a specified timeout period, the sender retransmits the frame.
- **Connection**: No formal logical connection is established between sender and receiver.
- **Application**: Used over unreliable wireless channels, such as Wi-Fi services.

### 5.3 Acknowledged Connection-Oriented Service
- **Mechanism**: A formal logical connection is established between sender and receiver before any data transfer begins.
- **Guaranteed Delivery**: Transmitted frames are assigned sequential numbers to guarantee that every frame is received exactly once, without loss or duplication, and in correct order.
- **Release**: The connection is formally released after data transfer is completed.

---

## 6. Topic 5: Data Link Layer Protocols

### 6.1 Protocol Classification and Taxonomy
Data Link Control protocols are categorized based on channel characteristics into **Noiseless Channel Protocols** and **Noisy Channel Protocols**.

**[📌 REFER TO PDF DIAGRAM: Taxonomy of Data Link Layer Protocols – PDF PAGE NO. 29]**

```
Data Link Layer Protocols
├── For Noiseless Channels
│   ├── Simplest Protocol
│   └── Stop-and-Wait Protocol
└── For Noisy Channels (ARQ)
    ├── Stop-and-Wait ARQ
    ├── Go-Back-N ARQ
    └── Selective Repeat ARQ
```

---

### 6.2 Protocols for Noiseless Channels

Noiseless channel protocols assume an ideal transmission medium with zero bit errors and no lost frames.

#### 5.2.1 Simplest Protocol
- **Characteristics**: Unidirectional data transmission with **no flow control** and **no error control**.
- **Assumption**: The receiver processing speed is infinitely fast; the receiver handles any incoming frame immediately without buffer overflow.
- **Sender Operation**: Fetches data from Network Layer, constructs a frame, and transmits it across the Physical Layer.
- **Receiver Operation**: Receives the frame from Physical Layer, strips the header, and immediately hands the payload to its Network Layer.

**[📌 REFER TO PDF DIAGRAM: Design of the Simplest Protocol – PDF PAGE NO. 30]**  
**[📌 REFER TO PDF DIAGRAM: Flow Diagram for the Simplest Protocol – PDF PAGE NO. 31]**

#### 5.2.2 Stop-and-Wait Protocol
- **Characteristics**: Unidirectional data frame transmission with **flow control**, but no error control.
- **Problem Addressed**: If frames arrive at the receiver faster than they can be processed, memory buffer overflow causes frame losses.
- **Flow Control Mechanism**: The sender transmits **one frame** and then stops, waiting for an explicit **Acknowledgement (ACK)** frame from the receiver before sending the next frame.
- **End of Transmission**: When transmission is complete, the sender transmits an **End of Transmission (CEOT)** frame.

**[📌 REFER TO PDF DIAGRAM: Design of the Stop-and-Wait Protocol – PDF PAGE NO. 33]**  
**[📌 REFER TO PDF DIAGRAM: Flow Diagram of the Stop-and-Wait Protocol – PDF PAGE NO. 35]**

---

### 6.3 Protocols for Noisy Channels (Automatic Repeat Request - ARQ)

Noisy channel protocols incorporate **error control** mechanisms using **Automatic Repeat Request (ARQ)**.

#### 5.3.1 Stop-and-Wait ARQ
- **Operating Steps**:
  1. **Error Detection**: The receiver uses a frame **checksum** to detect damaged frames. Damaged frames are silently discarded.
  2. **Retransmission**: The sender keeps a copy of the sent frame and starts a **retransmission timer**. If an ACK is not received before timeout, the sender retransmits the frame.
  3. **Sequence Numbering**: To prevent duplicate frame processing at the receiver (caused by lost or delayed ACKs), frames and ACKs are assigned sequence numbers using a **Modulo-2 numbering scheme** ($0$ and $1$).
  4. **ACK Identification**: ACKs are explicitly labeled as $ACK_0$ (acknowledging receipt of Frame 1 and expecting Frame 0 next) or $ACK_1$ (acknowledging receipt of Frame 0 and expecting Frame 1 next).

**[📌 REFER TO PDF DIAGRAM: Normal Operation of Stop-and-Wait ARQ Protocol – PDF PAGE NO. 38]**  
**[📌 REFER TO PDF DIAGRAM: Stop-and-Wait ARQ Operation with Lost Frame and Retransmission – PDF PAGE NO. 39]**

#### 5.3.2 Go-Back-N ARQ
- **Pipelining Concept**: Allows the sender to transmit multiple frames before receiving an ACK, keeping the transmission link busy and improving efficiency.
- **Sliding Window**: The sender maintains a sending window of size $N$, while the receiver window size is strictly $1$.
- **Sequence Numbers**: If $m$ bits are allocated in the frame header for sequence numbers, the sequence numbers range from $0$ to $2^m - 1$.
  - *Example*: For $m = 2$, sequence numbers are $0, 1, 2, 3, 0, 1, 2, 3 \dots$
- **Retransmission Behavior**: Go-Back-N uses **cumulative acknowledgements**. If a frame is lost or corrupted, the receiver discards it and all subsequent out-of-order frames. Upon timeout, the sender must **go back and retransmit all unacknowledged frames** starting from the lost frame.

**[📌 REFER TO PDF DIAGRAM: Go-Back-N ARQ Sliding Window and Frame Flow – PDF PAGE NO. 41]**

#### 5.3.3 Selective Repeat ARQ
- **Efficiency Improvement**: Resolves the inefficiency of Go-Back-N ARQ by retransmitting **only the specific lost or damaged frame**.
- **Receiver Buffering**: The receiver window size is greater than $1$. Out-of-order frames received correctly are stored in a receiver buffer rather than discarded.
- **Selective Retransmission**: The sender retransmits only the frame whose specific timer expires.

---

### 6.4 Piggybacking Technique
In real-world networks, communication is bidirectional (data flows simultaneously from Node A to Node B and from Node B to Node A).

> **Key Definition:** **Piggybacking** is a technique where control information (such as frame acknowledgements) is attached directly to outgoing data frames traveling in the opposite direction.

#### Advantages of Piggybacking:
1. Significantly reduces network overhead by eliminating dedicated ACK frames.
2. Increases channel bandwidth utilization efficiency.

*Requirement*: Both communicating nodes must run the same bidirectional protocol algorithm.

---

## 7. Numerical Concepts & Step-by-Step Solved Examples

### Solved Example 1: Bit Length Calculation
**Problem:** Calculate the bit length on a transmission medium if the propagation speed is $2 \times 10^8\text{ m/s}$ and the bit rate is $1\text{ Mbps}$ ($10^6\text{ bps}$).

**Solution Steps:**
1. Compute bit duration:
   $$\text{Bit Duration} = \frac{1}{\text{Bit Rate}} = \frac{1}{10^6\text{ bps}} = 10^{-6}\text{ seconds}$$
2. Calculate bit length using the formula $\text{Bit Length} = \text{Propagation Speed} \times \text{Bit Duration}$:
   $$\text{Bit Length} = (2 \times 10^8\text{ m/s}) \times 10^{-6}\text{ s} = 200\text{ meters}$$

---

### Solved Example 2: Decibel ($dB$) Attenuation Calculation
**Problem:** A signal travels through a transmission medium and its power is reduced to half ($P_2 = 0.5 P_1$). Calculate the signal attenuation in decibels ($dB$).

**Solution Steps:**
1. Apply the decibel formula $dB = 10 \log_{10} \left( \frac{P_2}{P_1} \right)$:
   $$dB = 10 \log_{10} \left( \frac{0.5 P_1}{P_1} \right) = 10 \log_{10}(0.5)$$
2. Evaluate logarithm: $\log_{10}(0.5) \approx -0.3010$.
3. Calculate result:
   $$dB = 10 \times (-0.3010) = -3.01\text{ dB}$$
   *(The negative sign confirms a $3\text{ dB}$ attenuation loss).*

---

### Solved Example 3: Nyquist Maximum Bit Rate (From PDF Page 19)
**Problem Statement in PDF:**
> Consider a noiseless channel with a bandwidth of $3000\text{ Hz}$ transmitting a signal with four signal levels ($L = 4$). Calculate the maximum bit rate.

**Step-by-Step Solution:**
1. **Identify Given Data**:
   - Bandwidth ($B$) = $3000\text{ Hz}$
   - Signal levels ($L$) = $4$
2. **Formula**:
   $$\text{Bit Rate} = 2 \times \text{Bandwidth} \times \log_2(L)$$
3. **Substitution**:
   $$\text{Bit Rate} = 2 \times 3000 \times \log_2(4)$$
4. **Logarithmic Calculation**:
   $$\log_2(4) = 2$$
5. **Final Multiplication**:
   $$\text{Bit Rate} = 2 \times 3000 \times 2 = 12000\text{ bps} \quad (12\text{ Kbps})$$

---

### Solved Example 4: Shannon Channel Capacity (From PDF Page 20)
**Problem Statement in PDF:**
> Consider an extremely noisy channel in which the value of the Signal-to-Noise Ratio ($SNR$) is almost zero. Calculate the channel capacity $C$.

**Step-by-Step Solution:**
1. **Identify Given Data**:
   - $SNR \approx 0$
2. **Formula**:
   $$C = B \log_2(1 + SNR)$$
3. **Substitution**:
   $$C = B \log_2(1 + 0) = B \log_2(1)$$
4. **Logarithmic Calculation**:
   $$\log_2(1) = 0$$
5. **Final Multiplication**:
   $$C = B \times 0 = 0\text{ bps}$$
   *(Conclusion: Zero signal quality yields zero data capacity regardless of bandwidth).*

---

### Solved Example 5: Go-Back-N Sequence Number Range (From PDF Page 40)
**Problem Statement in PDF:**
> If $m = 2$ bits are allocated in the frame header for sequence numbers in Go-Back-N ARQ, determine the range and sequence of frame numbers.

**Step-by-Step Solution:**
1. Apply range formula $0$ to $2^m - 1$:
   $$\text{Range} = 0 \text{ to } (2^2 - 1) = 0 \text{ to } (4 - 1) = 0 \text{ to } 3$$
2. Sequence sequence repeat pattern:
   $$0, 1, 2, 3, 0, 1, 2, 3, 0, 1, 2, 3 \dots$$

---

## 8. Important Definitions & Terminology Summary

- **Open System**: A set of standardized communication protocols permitting interaction between heterogeneous computing systems regardless of underlying hardware.
- **Peer-to-Peer Process**: Communication processes executing at the identical layer on two different network machines.
- **Interface**: The functional boundary and set of operational services defined between adjacent layers in a layered networking model.
- **Data Encapsulation**: The process of prepending control headers (and trailers) to higher-layer payloads as data descends the protocol stack.
- **Bit Length**: The physical distance occupied by a single binary bit on a transmission medium ($\text{Propagation Speed} \times \text{Bit Duration}$).
- **Attenuation**: Loss of signal energy and amplitude caused by the electrical resistance of the transmission medium.
- **Distortion**: Alteration of a composite signal's waveform caused by differing propagation delays across its constituent frequency components.
- **Signal-to-Noise Ratio (SNR)**: The ratio of average signal power to average noise power ($\frac{P_{\text{signal}}}{P_{\text{noise}}}$).
- **Framing**: The Data Link Layer function of dividing network bitstreams into discrete manageable data units bounded by delimiters.
- **Byte Stuffing**: Inserting an Escape byte (`ESC`) before data bytes that match control flags in character-oriented protocols.
- **Bit Stuffing**: Inserting a non-information `0` bit after five consecutive `1` bits in bit-oriented protocols to prevent flag pattern confusion (`01111110`).
- **Automatic Repeat Request (ARQ)**: An error-control strategy using ACKs, checksums, and timeouts to trigger automatic retransmissions.
- **Piggybacking**: Attaching control acknowledgements onto outgoing user data frames in bidirectional transmission.

---

## 9. Comprehensive Comparison Tables

### Table 1: OSI Upper Layers vs Lower Layers
| Feature / Parameter | Upper Layers (Layers 5–7) | Lower Layers (Layers 1–4) |
| :--- | :--- | :--- |
| **Included Layers** | Application (7), Presentation (6), Session (5) | Transport (4), Network (3), Data Link (2), Physical (1) |
| **Primary Focus** | Software application services & data formatting | Network data routing, transport & bit delivery |
| **Orientation** | User/Application-oriented | Network/Infrastructure-oriented |
| **Intermediate Nodes** | Absent on intermediate switches/routers | Processed at intermediate router nodes (Layers 1–3) |

### Table 2: Analog Signals vs Digital Signals
| Parameter | Analog Signals | Digital Signals |
| :--- | :--- | :--- |
| **Waveform Type** | Continuous electromagnetic wave | Discrete square wave pulses |
| **Signal Levels** | Infinitely many intensity levels | Limited discrete values (e.g., $0\text{ V}$ and $+V$) |
| **Noise Susceptibility** | High susceptibility to distortion | Lower susceptibility, easier noise filtering |
| **Representation** | Continuous data (e.g., human voice) | Binary sequence ($0$s and $1$s) |

### Table 3: Types of Transmission Impairments
| Impairment | Cause | Effect | Primary Mitigation |
| :--- | :--- | :--- | :--- |
| **Attenuation** | Transmission medium electrical resistance | Loss of signal power over distance | Amplifiers / Repeaters |
| **Distortion** | Differing propagation delays in composite signals | Change in signal waveform shape | Equalization filters |
| **Noise** | Thermal, Crosstalk, Impulse, Intermodulation | Unwanted random energy fluctuations | Shielding / Filtering |

### Table 4: Fixed-Size Framing vs Variable-Size Framing
| Parameter | Fixed-Size Framing | Variable-Size Framing |
| :--- | :--- | :--- |
| **Frame Boundaries** | Implicitly defined by fixed size | Explicitly defined by Start/End Flags |
| **Delimiters Required** | None | Character flags or bit flag patterns |
| **Framing Overhead** | Minimal overhead | Overhead from byte/bit stuffing |
| **Example** | ATM WAN Cells (53 Bytes) | Character-Oriented & Bit-Oriented Protocols |

### Table 5: Byte Stuffing vs Bit Stuffing
| Feature | Byte Stuffing (Character-Oriented) | Bit Stuffing (Bit-Oriented) |
| :--- | :--- | :--- |
| **Data Unit** | 8-bit character / byte | Continuous bit stream |
| **Delimiter Flag** | 8-bit character Flag | Special bit pattern `01111110` |
| **Stuffed Element** | Entire 8-bit `ESC` byte | Single `0` bit |
| **Trigger Rule** | Flag or `ESC` byte present in payload | Five consecutive `1`s present in payload |

### Table 6: Comparison of Data Link Control Protocols
| Protocol | Channel Type | Flow Control | Error Control | Retransmission Strategy | Efficiency |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Simplest** | Noiseless | No | No | None | Low |
| **Stop-and-Wait** | Noiseless | Yes (ACKs) | No | None | Low |
| **Stop-and-Wait ARQ** | Noisy | Yes | Yes | Retransmit single frame upon timeout | Low |
| **Go-Back-N ARQ** | Noisy | Yes (Sliding Window) | Yes | Retransmit all unACKed frames from lost frame | Medium |
| **Selective Repeat ARQ** | Noisy | Yes (Sliding Window) | Yes | Retransmit ONLY the specific lost frame | High |

---

## 10. Quick-Reference List of PDF Diagrams

For quick reference during examination revision, all relevant diagrams from `02_SECOND-CHAPTER.pdf` are listed below alongside their exact PDF viewer page numbers:

1. **[📌 REFER TO PDF DIAGRAM: Seven Layers of the OSI Model – PDF PAGE NO. 01]**
2. **[📌 REFER TO PDF DIAGRAM: Devices and Intermediate Nodes in OSI Communication – PDF PAGE NO. 03]**
3. **[📌 REFER TO PDF DIAGRAM: The Interface Between Layers in the OSI Model – PDF PAGE NO. 05]**
4. **[📌 REFER TO PDF DIAGRAM: Data Encapsulation and Decapsulation Process – PDF PAGE NO. 07]**
5. **[📌 REFER TO PDF DIAGRAM: Analog Signal vs Digital Signal Waveforms – PDF PAGE NO. 10]**
6. **[📌 REFER TO PDF DIAGRAM: Causes of Transmission Impairment – PDF PAGE NO. 12]**
7. **[📌 REFER TO PDF DIAGRAM: Attenuation and Amplification of Signals – PDF PAGE NO. 12]**
8. **[📌 REFER TO PDF DIAGRAM: Distortion in Composite Signals – PDF PAGE NO. 13]**
9. **[📌 REFER TO PDF DIAGRAM: Physical Layer Function and Bit Transmission – PDF PAGE NO. 17]**
10. **[📌 REFER TO PDF DIAGRAM: A Frame in a Character-Oriented Protocol & Byte Stuffing – PDF PAGE NO. 23]**
11. **[📌 REFER TO PDF DIAGRAM: A Frame in a Bit-Oriented Protocol & Bit Stuffing Mechanism – PDF PAGE NO. 24]**
12. **[📌 REFER TO PDF DIAGRAM: Bit Stuffing and Unstuffing Data Stream Example – PDF PAGE NO. 25]**
13. **[📌 REFER TO PDF DIAGRAM: Taxonomy of Data Link Layer Protocols – PDF PAGE NO. 29]**
14. **[📌 REFER TO PDF DIAGRAM: Design of the Simplest Protocol – PDF PAGE NO. 30]**
15. **[📌 REFER TO PDF DIAGRAM: Flow Diagram for the Simplest Protocol – PDF PAGE NO. 31]**
16. **[📌 REFER TO PDF DIAGRAM: Design of the Stop-and-Wait Protocol – PDF PAGE NO. 33]**
17. **[📌 REFER TO PDF DIAGRAM: Flow Diagram of the Stop-and-Wait Protocol – PDF PAGE NO. 35]**
18. **[📌 REFER TO PDF DIAGRAM: Normal Operation of Stop-and-Wait ARQ Protocol – PDF PAGE NO. 38]**
19. **[📌 REFER TO PDF DIAGRAM: Stop-and-Wait ARQ Operation with Lost Frame and Retransmission – PDF PAGE NO. 39]**
20. **[📌 REFER TO PDF DIAGRAM: Go-Back-N ARQ Sliding Window and Frame Flow – PDF PAGE NO. 41]**

---

## 11. Chapter Summary & Exam Recap

- **OSI Framework**: Formulated by ISO in 1947. 7 layers organized into Upper Layers (Application, Presentation, Session) and Lower Layers (Transport, Network, Data Link, Physical). Encapsulation appends headers ($H_7 \dots H_2$) and trailer ($T_2$); decapsulation strips them.
- **Physical Layer Metrics**: $Bit\ Length = Propagation\ Speed \times Bit\ Duration$. Signal impairments include Attenuation (energy loss, fixed with amplifiers), Distortion (composite signal delay differences), and Noise (Thermal, Intermodulation, Crosstalk, Impulse).
- **Channel Capacity Limits**:
  - Nyquist (Noiseless): $Bit\ Rate = 2 \times B \times \log_2(L)$
  - Shannon (Noisy): $Capacity = B \times \log_2(1 + SNR)$
- **Framing & Stuffing**:
  - Character-Oriented: Byte stuffing inserts `ESC` byte before payload `Flag` or `ESC`.
  - Bit-Oriented: Bit stuffing inserts `0` bit after five consecutive `1`s to avoid `01111110` flag confusion.
- **Data Link Control Protocols**:
  - Noiseless: Simplest (no flow/error control), Stop-and-Wait (flow control via ACKs).
  - Noisy (ARQ): Stop-and-Wait ARQ (Modulo-2 ACKs/timers), Go-Back-N ARQ (pipelining, sliding window $N$, retransmits all frames from lost frame), Selective Repeat ARQ (retransmits only lost frame).
  - Piggybacking attaches ACKs to outgoing data frames in bidirectional links.
