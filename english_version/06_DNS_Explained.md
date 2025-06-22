# Ultimate Guide to DNS (Domain Name System)

## 1. DNS Basics

### 1.1 What is DNS
- **Definition:** Domain Name System, a distributed database system that converts domain names to IP addresses
- **Function:** Enables two-way resolution between domain names and IP addresses
- **Features:**
  - Distributed architecture
  - Hierarchical domain name space
  - Uses C/S model
  - Uses UDP/TCP protocol (port 53)

### 1.2 Domain Name Structure
- **Hierarchy:** Right to left, from high to low
  - Root domain (.)
  - Top-level domain (TLD): .com, .org, .net, .cn, etc.
  - Second-level domain: baidu, google, etc.
  - Third-level domain: www, mail, blog, etc.
- **Example:** www.baidu.com.
  - Root domain: . (usually omitted)
  - TLD: .com
  - Second-level: baidu
  - Third-level: www

### DNS Client Program
DNS client is the software component that sends queries to DNS servers and receives responses, usually built into the OS or network applications.

#### Main Functions of DNS Client
 - Domain name resolution: Converts hostname to IP address
 - Reverse resolution: Converts IP address to hostname
 - Cache management: Maintains local DNS cache
 - Retry mechanism: Retries queries on timeout or failure
 - Server selection: Chooses among multiple DNS servers

## 2. DNS Resolution Process

### 2.1 Full Resolution Process (e.g., visiting www.example.com)

#### Local Query Phase (0~5ms)
1. **Browser cache query**
   - Checks browser DNS cache
   - Cache time determined by TTL
   - If hit, returns IP directly
2. **OS cache query**
   - Checks OS DNS cache
   - Windows: `ipconfig /displaydns`
   - Linux: `systemd-resolve --statistics`
   - If hit, returns IP directly
3. **hosts file query**
   - Checks local hosts file
   - Windows: `C:\Windows\System32\drivers\etc\hosts`
   - Linux: `/etc/hosts`
   - If hit, returns IP directly
4. **Local DNS server query**
   - Checks router DNS cache
   - Checks ISP DNS server cache
   - If hit, returns IP directly

#### Recursive Query Phase (typically 50~300ms)
1. **Root name server query**
   - Recursive server queries root server
   - Gets .com TLD server address
   - Example: a.gtld-servers.net
   - Typical response: 20-50ms
2. **TLD server query**
   - Recursive server queries TLD server
   - Gets example.com authoritative server address
   - Example: ns1.cloudflare.com
   - Typical response: 30-80ms
3. **Authoritative server query**
   - Recursive server queries authoritative server
   - Gets www.example.com A record
   - Example: 93.184.216.34
   - Typical response: 20-100ms

#### Response Return Phase
1. **Recursive server processing**
   - Validates returned IP
   - Checks DNS record validity
   - Updates local cache
2. **Client receives**
   - Recursive server returns IP to client
   - Client updates local cache
   - Cache time determined by TTL
3. **Establish connection**
   - Client uses obtained IP
   - Establishes TCP connection
   - Starts HTTP request

### 2.2 Query Methods

| Comparison | Recursive Query | Iterative Query |
|------------|----------------|----------------|
| Responsibility | DNS server must return final answer | DNS server can return a "hint" (other server address) |
| Query load | All handled by recursive server | Client (or initial server) queries step by step |
| Typical scenario | Client queries local DNS server | Hierarchical queries between DNS servers |
| Response | Directly returns IP or error | May return next server's address |

### 2.3 DNS Resolution Flowcharts

<!-- All flowcharts omitted -->

#### Real-World Hybrid Mode
**1. Hybrid Query (most common)**
 - Client → Recursive DNS server: Client initiates recursive query (requests final answer).
 - Recursive DNS server → Other DNS servers: Recursive server performs iterative queries on behalf of client, aggregates result and returns.

**2. Example (user visits website)**
 - User enters www.example.com in browser, OS queries local DNS server (e.g., 8.8.8.8) recursively.
 - Local DNS server queries root, TLD, and authoritative servers iteratively.
 - Local DNS server returns final IP to client via recursive response.

### 2.4 DNS Caching Mechanism
- Multi-level cache: browser → OS → router → recursive server
- Each level checks cache before querying next
- Cache time determined by TTL

## 3. DNS Record Types

### 3.1 Common Record Types
| Type  | Function         | Typical Scenario         | Example Record                                 |
|-------|------------------|-------------------------|------------------------------------------------|
| A     | IPv4 address     | Main domain resolution  | `@ A 192.0.2.1`                               |
| AAAA  | IPv6 address     | IPv6-enabled websites   | `www AAAA 2001:db8::1`                        |
| CNAME | Domain alias     | CDN, service migration  | `blog CNAME myblog.medium.com`                |
| MX    | Mail server      | Enterprise mail         | `@ MX 10 mail.example.com`                    |
| TXT   | Text info        | SPF, SSL cert verify    | `@ TXT "v=spf1 include:_spf.google.com ~all"` |
| NS    | Authoritative NS | Domain hosting transfer | `@ NS ns1.dnspod.com`                         |
| SRV   | Service locator  | VoIP, IM                | `_sip._tcp SRV 10 60 5060 sipserver.example.com` |

### 3.2 Special Records
- **PTR:** Reverse lookup (IP→domain)
- **SRV:** Service record
- **CAA:** Certificate Authority Authorization

## 4. DNS Server Types

### 4.1 By Function
- **Root name server:** 13 global root clusters
- **TLD server:** Manages top-level domains
- **Authoritative server:** Manages specific domains
- **Recursive server:** Provides DNS resolution service

### 4.2 By Deployment
- **Public DNS:**
  - Google DNS: 8.8.8.8
  - Cloudflare: 1.1.1.1
  - AliDNS: 223.5.5.5
- **ISP DNS:**
  - China Telecom: 114.114.114.114
  - China Unicom: 119.29.29.29
  - China Mobile: 211.136.192.6

## 5. DNS Security

### 5.1 Common Attacks
- **DNS hijacking:** Modifies DNS results
- **DNS amplification attack:** Uses DNS responses for DDoS
- **DNS tunneling:** Transfers other protocol data via DNS
- **DNS cache poisoning:** Pollutes DNS cache

### 5.2 Security Measures
- **DNSSEC:** DNS Security Extensions
- **DNS over HTTPS/TLS:** Encrypted DNS queries
- **DNS filtering:** Blocks malicious domains
- **DNS monitoring:** Detects abnormal queries 