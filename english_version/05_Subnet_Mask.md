# Subnet Mask

> **Subnet Mask** is a key tool for **dividing the network and host parts of an IP address**. It determines which **subnet** an IP address belongs to and helps routers decide whether a packet should be sent to the local network or to an external network.

## Why Do We Need Subnet Masks?

- **IP Address Classification Problem:** IPv4 addresses are divided into Class A, B, and C (e.g., 192.168.1.1), but fixed classes are inflexible, causing waste or shortage of IPs.
- **Improved IP Utilization:** Subnetting allows more precise IP allocation, reducing waste.
- **Optimized Network Management:** Different departments/areas can use independent subnets for easier management and security.

### Structure of an IP Address

An IPv4 address (e.g., 192.168.1.100) consists of two parts:

- **Network part (Network ID):** Identifies the network (e.g., 192.168.1)
- **Host part (Host ID):** Identifies the specific device in the network (e.g., .100)

> **The function of the subnet mask is to tell the computer which part of the IP address is the network and which part is the host.**

### Subnet Mask Notation

#### (1) Dotted Decimal Notation
Same format as IP address, e.g., 255.255.255.0.

- 255 means the part is a network bit
- 0 means the part is a host bit

Example:
- IP: 192.168.1.100
- Subnet mask: 255.255.255.0
- Network part: 192.168.1 (first 24 bits)
- Host part: .100 (last 8 bits)

#### (2) CIDR Notation (Classless Inter-Domain Routing)
A more concise way, e.g., /24 (equivalent to 255.255.255.0).

The number indicates the length of the network bits:
- /24 = first 24 bits are network bits (255.255.255.0)
- /16 = first 16 bits are network bits (255.255.0.0)

## How Does a Subnet Mask Work?

#### Example 1: Determine if Two IPs Are in the Same Subnet
IP1: 192.168.1.100, subnet mask 255.255.255.0
IP2: 192.168.1.200, subnet mask 255.255.255.0

**Steps:**
1. Convert IP and subnet mask to binary:
   - 192.168.1.100 → 11000000.10101000.00000001.01100100
   - 255.255.255.0 → 11111111.11111111.11111111.00000000
2. Perform bitwise AND (network part kept, host part zeroed):
   - 11000000.10101000.00000001.00000000 → 192.168.1.0 (Network ID)
3. Similarly, 192.168.1.200's network ID is also 192.168.1.0
**Conclusion:** The two IPs are in the same subnet and can communicate directly.

#### Example 2: IPs in Different Subnets
- IP1: 192.168.1.100, subnet mask 255.255.255.0 (Network ID: 192.168.1.0)
- IP2: 192.168.2.100, subnet mask 255.255.255.0 (Network ID: 192.168.2.0)
**Conclusion:** Not in the same subnet, need router forwarding.

## Subnetting

##### By adjusting the subnet mask, a large network can be divided into multiple smaller networks (subnets), improving IP utilization.

#### Example: Divide 192.168.1.0/24 into 4 subnets

**Original network:**
- IP range: 192.168.1.0 ~ 192.168.1.255
- Subnet mask: 255.255.255.0 (/24)
- Usable hosts: 254 (excluding all-0 and all-1 addresses)

**Divide into 4 subnets:**
- Need to borrow 2 host bits (because 2² = 4)
- New subnet mask: 255.255.255.192 (/26, first 26 bits are network bits)

**IP range for each subnet:**
1. Subnet 1: 192.168.1.0 ~ 192.168.1.63 (Network ID: 192.168.1.0)
2. Subnet 2: 192.168.1.64 ~ 192.168.1.127 (Network ID: 192.168.1.64)
3. Subnet 3: 192.168.1.128 ~ 192.168.1.191 (Network ID: 192.168.1.128)
4. Subnet 4: 192.168.1.192 ~ 192.168.1.255 (Network ID: 192.168.1.192)

> **Usable hosts per subnet: 2⁶ - 2 = 62**

#### Special Subnet Mask Addresses

- **255.255.255.255:** Broadcast address (to all devices)
- **0.0.0.0:** Default route (matches any IP)

## Default Subnet Masks (Classful Network)

In early classful networks, the subnet mask was determined by the IP class:

| IP Class | Default Subnet Mask | Example IP Range | Network Bits |
|----------|---------------------|------------------|--------------|
| Class A  | 255.0.0.0           | 1.0.0.1 ~ 126.255.255.254 | /8 |
| Class B  | 255.255.0.0         | 128.0.0.1 ~ 191.255.255.254 | /16 |
| Class C  | 255.255.255.0       | 192.0.0.1 ~ 223.255.255.254 | /24 |

Modern networks use CIDR (classless addressing) and no longer strictly rely on A/B/C classes.

## How to Calculate Subnet Masks?

#### Method 1: By Number of Hosts
**If you need at least N hosts:**
1. Find the smallest h such that 2ʰ - 2 ≥ N (h = number of host bits)
2. Subnet mask = 32 - h (CIDR notation)
- Example: Need 50 hosts:
    - 2⁶ - 2 = 62 ≥ 50 → h=6
    - Subnet mask = 32 - 6 = 26 (i.e., 255.255.255.192)

#### Method 2: By Number of Subnets
**If you need M subnets:**
1. Find the smallest s such that 2ˢ ≥ M (s = number of subnet bits)
2. New subnet mask = original mask bits + s
- Example: Original network 192.168.1.0/24, divide into 8 subnets:
    - 2³ = 8 → need 3 bits
    - New subnet mask = 24 + 3 = 27 (i.e., 255.255.255.224)

### Practical Applications

#### (1) Home Router
- Usually uses 192.168.1.0/24 (subnet mask 255.255.255.0)
- Usable IPs: 192.168.1.1 ~ 192.168.1.254

#### (2) Enterprise Network
Divided into multiple subnets, e.g.:
- Finance: 10.0.1.0/24
- Tech: 10.0.2.0/24
- Connected via router 