# TCP/IP Model vs OSI Seven-Layer Model
#### Why Do We Need Network Models?
Network communication involves complex hardware, software, and protocols. The purpose of models is:

- Layered design: Each layer focuses on specific functions and does not interfere with others (e.g., the physical layer handles cables, the application layer handles software).
- Standardization: Devices from different vendors can interoperate as long as they follow the same model (e.g., a Huawei router connects to an Apple phone).

## OSI Seven-Layer Model (Theoretical Reference Model)
Proposed by ISO (International Organization for Standardization), highly theoretical. In practice, TCP/IP is more commonly used, but OSI is the foundation for learning.

| Layer | Name         | Function Description                                 | Protocol Examples                  | Data Unit         | Analogy                        |
|-------|--------------|-----------------------------------------------------|-------------------------------------|-------------------|-------------------------------|
| 7     | Application  | User interface, provides network services           | HTTP, FTP, SMTP, DNS               | Data              | The content of a letter        |
| 6     | Presentation | Data format conversion (encryption, compression)    | SSL, JPEG, MPEG                    | Data              | Translating the letter         |
| 5     | Session      | Establish, manage, terminate sessions               | NetBIOS, RPC                       | Data              | Confirming communication       |
| 4     | Transport    | End-to-end reliable transmission (error/flow ctrl)  | TCP, UDP                           | Segment           | Post office confirms delivery  |
| 3     | Network      | Logical addressing (IP), routing                    | IP, ICMP, ARP, Router              | Packet            | Deciding the route             |
| 2     | Data Link    | Physical addressing (MAC), error detection          | Ethernet, PPP, Switch, MAC         | Frame             | Mailman delivers to address    |
| 1     | Physical     | Transmits bit streams (electrical/optical signals)  | RJ45, Fiber, WiFi, Hub             | Bit               | Paper and mail truck           |

#### OSI Model Workflow (Example: Accessing a Webpage)
- **Application Layer:** You enter http://www.example.com in your browser (HTTP protocol).
- **Presentation Layer:** HTTP request is encrypted (e.g., HTTPS) or compressed.
- **Session Layer:** Establishes session with server (e.g., Cookie maintains login).
- **Transport Layer (TCP):** Segments data, adds port number (e.g., 80), ensures reliable transmission.
- **Network Layer:** IP protocol encapsulates packet, adds source and destination IP (e.g., 192.168.1.100 → 93.184.216.34).
- **Data Link Layer:** Ethernet frames, adds MAC address (e.g., 00:1A:2B:3C:4D:5E).
- **Physical Layer:** Converts to electrical signals for transmission via cable/WiFi.

## TCP/IP Four-Layer Model (Practical Model)
Developed from real-world Internet evolution, removes OSI's session and presentation layers, more concise.

| Layer | Name             | Corresponding OSI Layer         | Function Description         | Protocol Examples                |
|-------|------------------|---------------------------------|-----------------------------|----------------------------------|
| 4     | Application      | Application + Presentation + Session | User interface & high-level protocols | HTTP, FTP, DNS, SMTP            |
| 3     | Transport        | Transport                       | End-to-end communication    | TCP, UDP                         |
| 2     | Network          | Network                         | Logical addressing & routing| IP, ICMP, ARP                    |
| 1     | Network Interface| Data Link + Physical            | Physical transmission & LAN | Ethernet, WiFi, PPP              |

#### TCP/IP Model Workflow (Example: Sending Email)
- **Application Layer:** Email client (e.g., Outlook) uses **SMTP** to prepare email content.
- **Transport Layer:** **TCP** segments the email, adds port number (e.g., 25), ensures data integrity.
- **Network Layer:** **IP** protocol encapsulates the packet, specifies sender and receiver IP (e.g., 192.168.1.100 → 142.250.190.46).
- **Network Interface Layer:** Sends data frame via **Ethernet/WiFi** to router, finally to the target server. 