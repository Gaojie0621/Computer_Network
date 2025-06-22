# Basic Network Communication Process

## 1. Overview of Network Devices

### 1.1 Main Device Functions
- **Switch:** Operates at the data link layer, forwards data frames based on MAC addresses
- **Router:** Operates at the network layer, routes based on IP addresses, usually integrates NAT
- **Server:** The target device providing network services

### 1.2 Basic Network Topology

<!-- Network topology diagram omitted -->

## 2. Outbound Communication Process (Client → Server)

### 2.1 Steps

1. **Application Layer Data Encapsulation**
   - Client application generates data
   - Adds transport layer header (TCP/UDP)
   - Adds network layer header (IP)
   - Adds data link layer header (Ethernet)

2. **Local Switch Processing**
   - Checks MAC address table
   - Forwards to corresponding port based on destination MAC address
   - If MAC address unknown, sends ARP broadcast

3. **Router NAT Translation**
   - Checks if destination IP is external
   - Performs source address translation: private IP → public IP
   - Records port mapping in NAT table

4. **Internet Routing**
   - Selects best path based on routing table
   - Forwards hop by hop to target network
   - Passes through multiple ISP routers

5. **Reaches Target Server**
   - Target network router receives packet
   - Forwards to target server
   - Server processes the request

### 2.2 Outbound Flow Diagram

<!-- Outbound flow SVG omitted -->

## 3. Inbound Communication Process (Server → Client)

### 3.1 Steps

1. **Server Response Processing**
   - Server application generates response data
   - Uses source address from request as destination address
   - Destination is client's public IP and port

2. **Server-Side Network Forwarding**
   - Through server-side switch
   - Through server-side router
   - Into the Internet

3. **Internet Return Routing**
   - Routes based on target public IP
   - Packet returns to client's network router

4. **Client Router Reverse NAT**
   - Checks NAT mapping table
   - Converts public address back to private address
   - Forwards to internal switch

5. **Reaches Client**
   - Switch forwards based on MAC address
   - Packet reaches client device

### 3.2 Inbound Flow Diagram

<!-- Inbound flow SVG omitted -->

## 4. Ports and Port Mapping

### 4.1 Port Concept

**Port** is a logical address used by transport layer protocols to identify different applications or services.

- **Port range:** 0-65535
- **Well-known ports:** 0-1023 (reserved)
- **Registered ports:** 1024-49151 (for applications)
- **Dynamic ports:** 49152-65535 (temporary)

### 4.2 Common Port Numbers

| Service | Port | Protocol | Description |
|---------|------|----------|-------------|
| HTTP    | 80   | TCP      | Web service |
| HTTPS   | 443  | TCP      | Secure web service |
| FTP     | 21   | TCP      | File transfer |
| SSH     | 22   | TCP      | Secure remote login |
| DNS     | 53   | UDP/TCP  | Domain name resolution |
| SMTP    | 25   | TCP      | Email sending |



### 4.3 How Port Mapping Works

1. **Outbound Mapping**
   - When an internal device initiates a connection, the router assigns a unique public port for each connection
   - Records: private IP:port ↔ public IP:port ↔ target IP:port in the NAT table

2. **Inbound Mapping**
   - When external response data arrives, the router checks the NAT table
   - Finds the corresponding internal address based on the public port
   - Forwards data to the correct internal device

3. **Connection Tracking**
   - Router maintains state info for each connection
   - Releases mapped port after connection ends
   - Port can be reassigned to new connections

## Key Knowledge Summary

### 1. Device Roles
- **Switch:** Layer 2 device, forwards by MAC, connects devices in the same subnet
- **Router:** Layer 3 device, routes by IP, connects different networks
- **NAT:** Address translation, allows internal devices to access the Internet

### 2. Communication Points
- **Outbound:** Internal → switch → router NAT → Internet → target server
- **Inbound:** Server → Internet → router reverse NAT → switch → internal device
- **Port mapping:** Different ports of one public IP correspond to different internal devices

### 3. Important Concepts
- **MAC address:** Data link layer address, used for forwarding within the same subnet
- **IP address:** Network layer address, used for routing across subnets
- **Port number:** Transport layer address, distinguishes different application services
- **NAT table:** Records address mappings, ensures correct data return 