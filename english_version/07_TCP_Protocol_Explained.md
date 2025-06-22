# Detailed Explanation of the TCP Protocol

## 1. TCP Overview

- **TCP (Transmission Control Protocol)** is a connection-oriented, reliable, byte-stream transport layer protocol.
- Used for: web browsing (HTTP/HTTPS), file transfer (FTP), email (SMTP/POP3), remote login (SSH), etc.
- **Key features:**
  - Connection-oriented (three-way handshake)
  - Reliable transmission (acknowledgment, retransmission, sequence numbers)
  - Byte stream (no message boundaries)
  - Flow control, congestion control

## 2. TCP Header Structure

| Field         | Length (bits) | Description                        |
|---------------|--------------|------------------------------------|
| Source Port   | 16           | Source application port            |
| Dest Port     | 16           | Destination application port       |
| Sequence Num  | 32           | Sequence number of first byte      |
| Ack Num       | 32           | Next expected byte (if ACK=1)      |
| Data Offset   | 4            | Header length (in 32-bit words)    |
| Reserved      | 3            | Reserved, must be 0                |
| Flags         | 9            | Control flags (SYN, ACK, FIN, etc.)|
| Window        | 16           | Flow control window size           |
| Checksum      | 16           | Error check                        |
| Urgent Ptr    | 16           | Urgent data pointer (if URG=1)     |
| Options       | variable     | Optional fields (MSS, SACK, etc.)  |
| Data          | variable     | Application data                   |

## 3. TCP Three-Way Handshake (Connection Establishment)

### 1. Purpose
- Establish a reliable connection before data transfer.
- Negotiate initial sequence numbers.

### 2. Steps
1. **SYN:**
   - Client sends SYN=1, seq=x to server (enters SYN_SENT state).
2. **SYN+ACK:**
   - Server replies with SYN=1, ACK=1, seq=y, ack=x+1 (enters SYN_RCVD state).
3. **ACK:**
   - Client sends ACK=1, ack=y+1 (enters ESTABLISHED state).
   - Server also enters ESTABLISHED state.

### 3. Why Three Handshakes?
- Prevents old duplicate connections: If an old SYN arrives late, the server requires client confirmation (third handshake), so the client can reject invalid connections.
- Synchronizes initial sequence numbers for both sides.

## 4. TCP Four-Way Handshake (Connection Termination)

### 1. Purpose
- Both sides confirm all data is sent.
- Safely close the bidirectional connection.

### 2. Steps
1. **FIN:**
   - Client sends FIN=1, seq=u (enters FIN_WAIT_1).
   - Means client has no more data to send, but can receive data.
2. **ACK:**
   - Server replies ACK=1, ack=u+1 (enters CLOSE_WAIT).
   - Client enters FIN_WAIT_2.
3. **FIN:**
   - Server sends FIN=1, seq=v (enters LAST_ACK).
4. **ACK:**
   - Client replies ACK=1, ack=v+1 (enters TIME_WAIT, waits 2MSL then CLOSED).
   - Server enters CLOSED after receiving ACK.

### 3. Key Points
- **Why four handshakes?**
  - TCP is full-duplex, both directions must be closed separately:
    - Client→Server (first FIN)
    - Server→Client (third FIN)
- **TIME_WAIT (waits 2MSL, usually 1~4 minutes):**
  - Ensures the last ACK reaches the server (if lost, server retransmits FIN).
  - Lets old packets in the network expire (prevents affecting new connections).

## 5. TCP Reliability Mechanisms

| Mechanism         | Principle                                                      |
|-------------------|---------------------------------------------------------------|
| Acknowledgment    | Receiver returns ACK=1, ack=n+1 to confirm data received.      |
| Retransmission    | Sender retransmits if no ACK (RTO is dynamically calculated).  |
| Sequence Number   | Each byte has a unique number for ordering and deduplication.  |
| Sliding Window    | Receiver advertises buffer size, controls sender's rate.       |
| Fast Retransmit   | Retransmit immediately after 3 duplicate ACKs (avoid timeout). |

## 6. Wireshark Packet Capture Examples

### Three-Way Handshake:
```
# Client → Server
[SYN] Seq=0 Win=64240 Len=0 MSS=1460
# Server → Client
[SYN, ACK] Seq=0 Ack=1 Win=29200 Len=0 MSS=1460
# Client → Server
[ACK] Seq=1 Ack=1 Win=64240 Len=0
```

### Four-Way Handshake:
```
# Client → Server
[FIN, ACK] Seq=1 Ack=1 Win=64240 Len=0
# Server → Client
[ACK] Seq=1 Ack=2 Win=29200 Len=0
# Server → Client
[FIN, ACK] Seq=1 Ack=2 Win=29200 Len=0
# Client → Server
[ACK] Seq=2 Ack=2 Win=64240 Len=0
```

## 7. UDP Protocol Overview

- **UDP (User Datagram Protocol)** is a connectionless, unreliable, lightweight transport layer protocol.
- Used for: real-time audio/video, online games, DNS queries, IoT, broadcast/multicast, etc.

### 1. UDP Header Structure
| Field         | Length (bits) | Description                |
|---------------|--------------|----------------------------|
| Source Port   | 16           | Source port                |
| Dest Port     | 16           | Destination port           |
| Length        | 16           | Total length (header+data) |
| Checksum      | 16           | Error check                |
| Data          | variable     | Application data           |

### 2. UDP Features
- No connection, no handshake
- No reliability, no retransmission, no ordering
- No flow/congestion control
- Lightweight, low overhead (8-byte header)
- Application layer can implement its own reliability if needed

### 3. UDP Application Scenarios
| Scenario            | Example Protocol | Why Use UDP                        |
|---------------------|-----------------|------------------------------------|
| Real-time audio/video | RTP/WebRTC      | Low latency, some loss is tolerable|
| Online games        | QUIC (some games)| Fast response, TCP retransmit causes lag|
| DNS queries         | DNS              | Small requests, low retry cost     |
| IoT sensor data     | MQTT-SN          | Low power, minimal protocol        |
| Broadcast/multicast | DHCP, IPTV       | TCP doesn't support multicast      |

### 4. UDP Pros and Cons
#### Pros
- **Low latency:** No handshake, direct transmission (good for games/streaming)
- **Low overhead:** Only 8-byte header, no connection state
- **Flexibility:** Application can define its own reliability (e.g., QUIC)
#### Cons
- **Unreliable:** App must handle loss, out-of-order
- **Vulnerable:** No state, easy for UDP flood attacks (needs firewall)
- **No congestion control:** Can worsen network congestion (app must limit rate)

### 5. Wireshark UDP Example
**UDP DNS Query:**
```
Source Port: 54321    Destination Port: 53
Length: 32            Checksum: 0x2a1b
Data: DNS Query for www.example.com
```
**Compare with TCP HTTP Request:**
```
Source Port: 49152    Destination Port: 80
Sequence Number: 1     Acknowledgment Number: 1
Header Length: 20      Flags: [ACK, PSH]
Window Size: 64240     Checksum: 0x5983
Data: HTTP GET / HTTP/1.1
```

---

## 8. Full Comparison: UDP vs TCP

| Aspect        | UDP                        | TCP                                 |
|--------------|----------------------------|-------------------------------------|
| Connection    | Connectionless             | Connection-oriented (3-way handshake)|
| Reliability   | No guarantee, no order     | ACK, retransmit, ordered delivery   |
| Flow Control  | None                       | Sliding window                     |
| Congestion    | None                       | Slow start, congestion avoidance    |
| Header Size   | 8 bytes (fixed)            | At least 20 bytes (with options)    |
| Efficiency    | High (no control)          | Lower (maintains state)             |
| Data Boundary | Preserved                  | Byte stream (no boundary)           |
| Multicast     | Supported                  | Not supported                       |
| Typical Use   | DNS, video, games          | HTTP, FTP, email                    |

---

### Advanced: Achieving Reliability on UDP

#### 1. QUIC Protocol (basis of HTTP/3)
- Developed by Google: Implements multiplexing, encryption, retransmission on UDP.
- **Advantages:**
  - Reduces handshake latency (0-RTT resume)
  - Avoids TCP head-of-line blocking (independent streams)

#### 2. Custom Reliable UDP
- **How:**
  - Add sequence/ACK (e.g., RTMP)
  - Forward error correction (FEC) to reduce retransmission 