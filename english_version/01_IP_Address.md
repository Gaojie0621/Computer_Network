# What is an IP Address?
> An IP address (Internet Protocol Address) is the unique identifier for a device on the Internet, similar to a "house number" in the real world, used for communication between devices.

IP addresses are divided into:

- IPv4 (e.g., 192.168.1.1, 32 bits, about 4.2 billion, nearly exhausted)
- IPv6 (e.g., 2001:0db8:85a3::8a2e:0370:7334, 128 bits, almost unlimited)

## 1. Public IP vs Private IP
#### **Public IP**
What is it: Like your home's "mailing address" in the real world, globally unique. For example, 8.8.8.8 (Google's DNS server).

**Role:** A public IP is the unique identifier for a device on the Internet. Any device that needs to be accessed directly (such as a web server) must have a public IP.

**Features:**
- Limited quantity (only about 4.2 billion IPv4 addresses), must be rented.
- Directly exposed to the Internet, may be attacked, usually protected by a firewall.

#### **Private IP**
What is it: Like your home's "room number" (e.g., living room, bedroom), only valid within the local network. Common private IP ranges:

192.168.x.x (e.g., your router's 192.168.1.1)
10.x.x.x, 172.16.x.x~172.31.x.x

**Role:** Devices in a LAN (e.g., home, company) use private IPs to communicate with each other, but cannot be accessed directly from the Internet.

**Features:**
- Free and reusable (your home and your neighbor's home can both use 192.168.1.1).
- Need NAT technology to access the Internet.

### 1.1 NAT (Network Address Translation)
**Why is NAT needed?**
Problem: Private IPs (e.g., your phone 192.168.1.100) cannot access the Internet directly because the Internet only recognizes public IPs.

Solution: The router uses NAT to "translate" your private IP into a public IP.

**How NAT works**
Scenario: You use your phone (192.168.1.100) to access Baidu (220.181.38.148).

**Steps:**
- The phone sends a request to the router: "I want to visit Baidu."
- The router replaces your private IP (192.168.1.100) with its public IP (e.g., 120.80.1.1) and records the connection (like a logbook).
- When Baidu replies, the router uses the logbook to forward the data to your phone.

**Types of NAT**
- One-to-one: A private IP is mapped to a fixed public IP (commonly used by enterprises).
- Many-to-one (PAT): Multiple devices share one public IP (common in home routers, distinguished by port numbers).

**Analogy**
Like a company front desk:
You (private IP) call a customer (Internet), the front desk (NAT router) uses the company's main line (public IP) to call for you. When the customer calls back, the front desk forwards the call to you based on the extension number.

### 1.2 DNS Resolution
**Why is DNS needed?**
Problem: Humans can't remember IPs (e.g., 220.181.38.148), but can remember domain names (e.g., www.baidu.com).

Solution: DNS translates domain names into IPs, like a "phone book."

**DNS Resolution Process**
- Enter website: You type www.baidu.com in your browser.
- Local query:
    - Check browser cache → computer cache → router cache.
    - If not found, ask the local DNS server (usually provided by ISP, e.g., 114.114.114.114).
- Recursive query:
    - Local DNS asks the root name server (13 groups worldwide): "Who manages .com?"
    - The root server returns the address of the .com top-level domain server.
    - Local DNS then asks the .com server: "Who manages baidu.com?"
    - Finally gets the IP for www.baidu.com (e.g., 220.181.38.148).
- Return result: The browser uses this IP to access Baidu's server.

**Analogy**
Like asking for directions:
You ask, "Where is Xinhua Bookstore?" (domain name).
The local guide (DNS) checks their notebook (cache) first, if not found, asks a higher-level navigation system (root name server).
Finally tells you "Xinhua Bookstore is at 123 Renmin Road" (IP address).

### 1.3 Example of the Whole Process
Suppose you use your home WiFi (private IP) to access Baidu:
- Your computer (192.168.1.100) asks DNS: "What is the IP of www.baidu.com?" → gets 220.181.38.148.
- The router uses NAT to change the source IP of your request packet from 192.168.1.100 to public IP 120.80.1.1 and sends it to Baidu.
- Baidu replies to 120.80.1.1, and the router forwards it to 192.168.1.100 based on the NAT record.
- You see the Baidu webpage.

### 1.4 IP Allocation in Home Networks
**(1) Router's IP**
- WAN port (external interface):
  When the router connects to the modem/broadband, the ISP assigns a public IP (or a "pseudo-public IP" from the ISP's internal network) to the router's WAN port.
    - If it's a real public IP: e.g., 120.80.1.1 (globally unique, can be accessed directly from the Internet).
    - If it's an ISP internal IP: e.g., 10.100.100.100 (shared by the ISP, increasingly common in homes, needs ISP NAT to access the Internet).

**(2) Home LAN Devices**
The router uses DHCP to automatically assign private IPs to connected devices (phones, computers, etc.), e.g.:
Your computer: 192.168.1.100
Your phone: 192.168.1.101
Router itself: 192.168.1.1 (gateway address)
These private IPs are only valid within the home LAN and cannot be accessed directly from the Internet.

#### Complete Home Internet Process (with NAT and DNS)
Suppose you use your computer to access www.baidu.com:
**1. DNS Resolution:**
- Computer asks DNS server: "What is the IP of www.baidu.com?" → gets 220.181.38.148.
**2. Send Request:**
- Computer (192.168.1.100) sends the packet to the router (192.168.1.1).
**3. NAT Translation:**
- Router changes the source IP of the packet from 192.168.1.100 to its public IP (e.g., 120.80.1.1) and records the connection (port mapping).
**4. Internet Transmission:**
- Baidu receives the request and replies to the router's public IP 120.80.1.1.
**5. NAT Reverse Translation:**
- Router forwards the reply to computer 192.168.1.100 based on the record.

## 2. Static IP vs Dynamic IP
### 2.1 Concepts
#### Static IP
**(1) Definition**
A static IP is manually configured and fixed, it does not change automatically.
**(2) Features**
- Permanence: Won't change unless manually modified.
- Manual configuration: Need to manually enter IP, subnet mask, gateway, etc. on the device or router.
Suitable for: Servers, network devices, surveillance cameras, etc. that require long-term stable access.
**(3) Advantages**
- Stability: Suitable for services that require long-term remote access (e.g., websites, VPN, NAS).
- Easy management: Fixed IPs make it easier for network admins to maintain and troubleshoot.
- Suitable for business use: Enterprise servers, mail servers, etc. must use static IPs.
**(4) Disadvantages**
- Complex management: Each device must be configured separately, easy to have conflicts (e.g., two devices set to the same IP).
- Higher cost: ISPs usually charge for static public IPs (home broadband generally does not provide static IPs).
- Security risk: Fixed IPs are easier targets for hackers, need extra protection.
**(5) Scenarios**
🌐 Server hosting (web/database servers)
📹 Surveillance systems (IP cameras, NVR)
🏢 Enterprise LAN devices (printers, access control)

#### Dynamic IP
**(1) Definition**
A dynamic IP is automatically assigned and may change, usually by a DHCP (Dynamic Host Configuration Protocol) server.
**(2) Features**
- Temporariness: May change after lease expires (e.g., after router reboot or DHCP renewal).
- Automatic acquisition: Devices get IPs from DHCP server automatically when connecting to the network (no manual setup needed).
Suitable for: Ordinary home devices (phones, computers, smart home).
**(3) Advantages**
- Plug and play: Devices get IPs automatically when connecting, no manual configuration needed.
- Saves IP resources: IPs can be reused (e.g., after a device disconnects, the IP can be assigned to a new device).
- Suitable for home users: No need for fixed IPs for normal Internet, video, gaming, etc.
**(4) Disadvantages**
- Instability: IP may change, not suitable for services that require long-term remote access.
- Depends on DHCP server: If DHCP fails, devices can't get IPs.
- Not suitable for servers: Dynamic IPs can't guarantee stable external access.
**(5) Scenarios**
🏠 Home network (phones, computers, smart TVs)
📱 Mobile devices (cafe, airport WiFi)
🖨️ Temporary devices (guest network, meeting room devices)

### 2.2 Relationship between Public/Private IP and Static/Dynamic IP
- **Public IP:** Globally unique Internet address, can be static or dynamic.
    - Home broadband usually assigns dynamic public IPs (may change after modem reboot).
    - Enterprises can apply for static public IPs (for a fee).
- **Private IP:** Internal LAN address (e.g., 192.168.x.x), can be static or dynamic.
    - Routers by default use DHCP to assign dynamic private IPs to phones, computers.
    - Can manually set static private IPs for printers, NAS, etc.

## Summary
Public IP: The "ID card" on the Internet, globally unique.
Private IP: The "house number" inside the LAN, reusable.
NAT: Technology to "disguise" private IPs as public IPs for Internet access.
DNS: The "translator" that converts domain names (e.g., www.baidu.com) to IPs. 