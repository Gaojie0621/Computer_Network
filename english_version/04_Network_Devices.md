# Detailed Network Devices

## 1. Physical Layer Devices (Layer 1)

### (1) Network Cable (Ethernet Cable, e.g., Cat5e/Cat6)
- **Function:** Transmits electrical signals (wired network)
- **Types:**
  - Straight-through: Connects different devices (e.g., PC ↔ Switch)
  - Crossover: Connects similar devices (e.g., PC ↔ PC)
- **Typical Scenarios:** Home/enterprise LAN cabling

### (2) Optical Fiber
- **Function:** Transmits data via optical signals (high speed, long distance)
- **Types:**
  - Single-mode fiber (long distance, e.g., ISP backbone)
  - Multi-mode fiber (short distance, e.g., data center)

### (3) Repeater
- **Function:** Amplifies signals, extends transmission distance (solves signal attenuation)
- **Modern Alternatives:** Optical fiber or switch (repeaters are rarely used now)

### (4) Hub
- **Function:** Broadcasts data to all ports (shared bandwidth)
- **Disadvantage:** Large collision domain, low efficiency (replaced by switches)

## 2. Data Link Layer Devices (Layer 2)

### (1) NIC (Network Interface Card)
- **Function:** Hardware interface for devices to connect to the network (e.g., PC Ethernet card, WiFi module)
- **Core Functions:**
  - Data encapsulation/decapsulation: Converts upper layer (e.g., TCP/IP) packets to physical layer signals (electrical/optical/wireless)
  - Media access control: Follows Ethernet, WiFi, etc. protocol rules for sending/receiving
  - Unique identifier: Ensures device uniqueness in LAN via MAC address
- **Key Parameters & Technical Details:**
  | Parameter/Tech | Description |
  |---------------|-------------|
  | MAC Address | 48-bit physical address (e.g., 00:1A:2B:3C:4D:5E), assigned by IEEE, first 24 bits are vendor OUI |
  | Speed | 10/100/1000 Mbps (Gigabit), 10 Gbps, etc., must match switch port |
  | Interface Type | RJ45 (Ethernet), SFP (fiber), PCIe (internal), USB (external) |
  | Mode | Full duplex (simultaneous send/receive), half duplex (alternating, obsolete) |
  | Offload Tech | e.g., TCP Offload (TOE), hardware handles part of protocol stack, reduces CPU load |
- **Types:**
  - Wired NIC:
    - Integrated: On motherboard (e.g., Intel I219-V)
    - Standalone: PCIe expansion card (for servers/high performance)
    - Fiber NIC: Connects via SFP module (e.g., 10G SFP+)
  - Wireless NIC:
    - WiFi card (supports 802.11ac/ax)
    - Bluetooth adapter (sometimes integrated)
- **Typical Applications:**
  - PC: Integrated gigabit Ethernet card (e.g., Realtek RTL8168)
  - Data center server: 10G/25G fiber NIC (e.g., Mellanox ConnectX-6)
  - Industrial: Industrial-grade NIC with isolation (e.g., Moxa PCIe-G105)

### (2) Switch
- **Function:** Forwards data based on MAC address (each port has independent bandwidth)
- **Types:**
  - Unmanaged: Plug and play (common in homes)
  - Managed: Supports VLAN, QoS (enterprise)
- **Advantage:** Isolates collision domains, improves LAN efficiency

## 3. Network Layer Devices (Layer 3)

### (1) Router
- **Function:** Cross-network forwarding (based on IP), connects different subnets or ISPs
- **How it works:**
  - **Data forwarding process (e.g., home network to Baidu):**
    1. Internal device sends request:
       - PC (192.168.1.100) sends packet to 220.181.38.148 (Baidu)
    2. Router processing:
       - Checks routing table: if not local subnet, forwards via default gateway (ISP public IP)
       - NAT: Changes source IP from 192.168.1.100 to public IP 120.80.1.1, records mapping
    3. ISP network transmission: Packet reaches Baidu via ISP
    4. Response: Baidu sends packet to 120.80.1.1, router forwards to internal PC
  - **Routing Table:**
    - Router's "navigation map", decides next hop
    - Example:
      | Destination | Subnet Mask | Next Hop | Interface |
      |-------------|-------------|----------|-----------|
      | 192.168.1.0 | 255.255.255.0 | - | LAN |
      | 0.0.0.0 | 0.0.0.0 | 203.0.113.1 | WAN |
    - Default route (0.0.0.0): All non-local traffic forwarded to ISP gateway
- **Functions:**
  - NAT (private IP to public IP)
  - DHCP (auto IP assignment)
  - Firewall (basic security)
- **Typical Scenarios:** Home broadband router, enterprise core router

### (2) Layer 3 Switch
- **Function:** Combines switch (Layer 2) and router (Layer 3) for high-speed LAN forwarding
- **Advantage:** Faster than traditional routers (hardware routing)
- **Application:** Data centers, enterprise LAN core

## 4. Higher Layer Devices (Layer 4~7)

### (1) Firewall
- **Function:** Filters illegal traffic, protects network security
- **Types:**
  - Hardware firewall (enterprise, e.g., Cisco ASA)
  - Software firewall (e.g., Windows Defender)

### (2) Load Balancer
- **Function:** Distributes traffic to multiple servers (improves performance/reliability)
- **Typical Protocols:** LVS (Linux Virtual Server), Nginx

### (3) VPN Gateway
- **Function:** Establishes encrypted tunnels for secure remote access (e.g., enterprise LAN)

### (4) Proxy Server
- **Function:** Relays client requests (hides real IP, caching)
- **Types:** Forward proxy (client config), reverse proxy (server config, e.g., Nginx)

## 5. Wireless Network Devices

### (1) Access Point (AP)
- **Function:** Broadcasts WiFi signal, connects wireless devices (phones, laptops)
- **Difference from Router:** AP only provides wireless access, no routing (needs router)

### (2) Wireless Router
- **Function:** Integrates router + switch + AP (common in homes)

### (3) Wireless Bridge
- **Function:** Connects two LANs wirelessly (replaces cable, e.g., remote camera access)

## 6. Other Key Devices

### (1) Modem
- **Function:** Converts analog ↔ digital signals (e.g., ADSL modem)

### (2) Optical Network Terminal (ONT)
- **Function:** Converts fiber ↔ Ethernet signals (required for home fiber broadband)

### (3) IDS/IPS (Intrusion Detection/Prevention System)
- **Function:** Monitors network attacks in real time (e.g., Snort, Suricata)

## 7. Device Comparison Summary

| Device | OSI Layer | Core Function | Typical Scenario |
|--------|-----------|---------------|------------------|
| Hub | L1 | Broadcast data (obsolete) | Early LAN |
| Switch | L2 | MAC-based forwarding | Enterprise/home LAN |
| Router | L3 | Cross-network IP routing | Connects ISP or subnets |
| Firewall | L3~L7 | Traffic filtering | Enterprise security |
| Wireless AP | L1~L2 | WiFi access | Mall/airport coverage |

---

#### Terminology

##### ISP (Internet Service Provider)
- **Definition:** Company/organization providing Internet access
- **Main Services:**
  - Broadband access (fiber, ADSL)
  - Mobile network (4G/5G)
  - Dedicated line (enterprise)
- **Typical Providers:**
  - China: China Telecom, China Unicom, China Mobile
  - International: AT&T, Verizon, Comcast
- **Functions:**
  - Provides public IP addresses
  - Maintains backbone network
  - Provides DNS service
  - Manages network bandwidth 