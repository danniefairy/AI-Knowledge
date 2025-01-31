**Network Communication Standards and Protocols**

## Introduction

A standard defines how to establish a network conversation, ensuring seamless communication between different devices and applications.

---

## **1. Application Layer**

The Application Layer provides services to end users and ensures applications can communicate effectively over a network.

### **Protocols:**

- **HTTP** (HyperText Transfer Protocol)
- **FTP** (File Transfer Protocol)
- **SSH** (Secure Shell)
- **[DHCP](https://github.com/danniefairy/AI-Knowledge/blob/main/Network.md#dhcp-extra-notes)** (Dynamic Host Configuration Protocol)
- **Proxy** (Intermediary Server Protocol)
- **[DNS](https://github.com/danniefairy/AI-Knowledge/blob/main/Network.md#dns-extra-notes-dns-extra-notes)** (Domain Name System)

Each protocol operates on different ports.

### **[SSL/TLS (After TCP Handshake Completion)](https://github.com/danniefairy/AI-Knowledge/blob/main/Network.md#subnet-extra-notes-subnet-extra-notes)**

SSL/TLS is used to secure connections after the TCP handshake has been completed and a connection is established.

---

## **2. Transport Layer**

The Transport Layer ensures end-to-end communication reliability and integrity.

### **Functions:**

- **Maintains End-to-End Communication**
- **Checksum Verification**

### **Protocols:**

1. **TCP (Transmission Control Protocol)**
   - One-to-One Communication
   - Used for protocols such as HTTP and FTP
2. **UDP (User Datagram Protocol)**
   - Many-to-Many Communication
   - Used for protocols such as DNS and DHCP

---

## **3. Network Layer**

The Network Layer handles packet transfers across network boundaries and ensures efficient routing.

### **Functions:**

- **Packet Transfer**
- **Checksum Verification**

### **Protocols:**

- **IP (Internet Protocol)**
- **[NAT](#nat-extra-notes)** (Network Address Translation)
- **ICMP (Internet Control Message Protocol, e.g., Ping)**
- **ARP (Address Resolution Protocol)**
  - ARP is responsible for mapping **IP addresses (logical)** to **MAC addresses (physical, provided by the Network Interface Card (NIC))**.

### **Features:**

#### **Firewall**

- Implements **Access Control Lists (ACL)** for security and traffic management.

#### **Router (Default Gateway)**

- Connects two different network protocols.
- Handles IP addressing and subnetting.
- Uses a **Routing Table** to exchange routing information between routers.
  - Example: **Open Shortest Path First (OSPF) using Dijkstra’s Algorithm**
- A **switch** only handles packet exchange and does not perform routing.

#### **[Subnet](https://github.com/danniefairy/AI-Knowledge/blob/main/Network.md#subnet-extra-notes-subnet-extra-notes)**

- Defines different ranges of subnet allocation.

#### **0.0.0.0 Address**

- **Server:** Listens to all ports on a local server.
- **Router:** Used as the default route.

---

## **4. Link Layer**

The Link Layer is responsible for handling node-to-node communication and defining how data is physically transmitted.

### **Functions:**

- **NIC (Network Interface Card) Provides MAC Addresses**
- **Handles Direct Node Connections**

---

## **5. Additional Notes on Each Component**

### **DNS Extra Notes** {#dns-extra-notes}

#### **FQDN (Fully Qualified Domain Name)**
- Example: `www.abc.com`
  - **Top-Level Domain (TLD):** `com`
  - **Second-Level Domain (SLD):** `abc`
  - **Third-Level Domain:** `www`

#### **A (AAAA) Record**
- Maps an IP address to a domain name:
  - `a.abc.com  A  192.168.1.1`

#### **CNAME Record**
- Maps different third-level domains to the same machine:
  - `a.abc.com  A  192.168.1.1`
  - `b.abc.com  CNAME  a.abc.com`
  - `c.abc.com  CNAME  a.abc.com`
  - Querying `b.abc.com` or `c.abc.com` returns `192.168.1.1`

#### **DNAME Record**
- Maps second-level domains:
  - `www.abc.com  A  192.168.1.1`
  - `def.com  DNAME  abc.com`
  - Querying `www.def.com` returns `192.168.1.1`

#### **MX Record**
- Maps an IP address of an email server to an email domain.

#### **How DNS Works:**
1. Server queries **DNS Resolver** (checks cache first).
2. If not found, queries **Root Name Server** (e.g., `.ca`).
3. Queries **Top-Level Domain (TLD) Name Server** (e.g., `.com.ca`).
4. Queries **Authoritative DNS Server** (contains A/AAAA and CNAME records, e.g., Route53 for `abc.com.ca`).
5. Returns the corresponding IP address.

#### **Monitoring DNS:**
1. Check cache on the **DNS Resolver**.
2. Check **Name Server Records**.
3. Check **SOA (Start of Authority) Record**:
   - The SOA record is updated whenever DNS records change.

#### **Reference:**
- [DNS Explanation](https://articles.onlinetoknow.com/dns/)

### **Subnet Extra Notes** {#subnet-extra-notes}

#### **Class-Based Subnetting:**

- **Class A:** Large networks
- **Class B:** Medium-sized networks
- **Class C:** Small networks
- **Class D:** Used for multicast transmissions

#### **CIDR (Classless Inter-Domain Routing):**

- Enables flexible subnetting.
- Examples:
  - **A = /8**
  - **B = /16**
  - **C = /24**

### **NAT Extra Notes** {#nat-extra-notes}

#### **Network Address Translation (NAT)**

- Allows internal subnet servers to communicate with external networks.

#### **SNAT (Source NAT):**

- Modifies the source IP address of outgoing packets.
- Example:
  - `ServerInSubnetA -> ServerWithPublicIPB -> ServerOutsideSubnetC (ipA->ipB, ipB->ipC)`
  - `ServerInSubnetA <- ServerWithPublicIPB <- ServerOutsideSubnetC (ipC->ipB, ipB->ipA)`

#### **DNAT (Destination NAT):**

- Modifies the destination IP address of incoming packets.
- Example:
  - `ServerInSubnetA <- ServerWithPublicIPB <- ServerOutsideSubnetC (ipC->ipB, ipB->ipA)`
  - `ServerInSubnetA -> ServerWithPublicIPB -> ServerOutsideSubnetC (ipA->ipB, ipB->ipC)`

### **DHCP Extra Notes**

#### **Dynamic Host Configuration Protocol (DHCP)**

- Dynamically assigns internal IP addresses within a subnet.

#### **Process:**

1. **Server requests an IP address.**
2. **DHCP server assigns an available address.**
3. **Unused addresses return to the DHCP pool.**

#### **Example Setup:**

```
[Server] -> Switch <- [DHCP Server]
```

### **SSL/TLS Extra Notes**
#### **Process:**
1. **Server Gets Digital Certificates**
   - Server creates public/private key.
   - Server provides public key and personal information to CA.
   - CA returns a digital certificate (encrypted by CA private key).
2. **Client (Browser) Connects to Server:**
   - Client sends an HTTPS request to the server.
   - Server sends a digital certificate and public key to the client.
   - Client checks with CA for the server's digital certificate (decrypts using CA public key in the browser).
   - Client creates a symmetric key.
   - Client encrypts the key with the server's public key and sends it to the server.
   - Server decrypts it with its private key.
   - Client and server now communicate with each other using data encrypted by the symmetric key.

#### **Types of Encryption:**
1. **Symmetric Encryption Algorithm:**
   - DES, AES
2. **Asymmetric Encryption Algorithm:**
   - RSA, DSA
3. **Hash Function:**
   - MD5, SHA1, SHA256

### **Proxy vs. Reverse Proxy vs. NAT**
#### **Proxy:**
- Acts as an intermediary between a client and a destination server.
- Used to filter, cache, and monitor client requests.
- Common use cases include web filtering and anonymous browsing.

#### **Reverse Proxy:**
- Sits between clients and backend servers, forwarding client requests to the appropriate server.
- Enhances security, load balancing, and caching.
- Examples: Nginx, Apache mod_proxy, AWS ALB.

#### **NAT (Network Address Translation):**
- Enables multiple devices in a private network to share a single public IP.
- **SNAT (Source NAT):** Translates the private IP of outgoing packets to a public IP.
- **DNAT (Destination NAT):** Redirects incoming traffic to a specific private IP within the network.


---

## **Conclusion**

Understanding these network layers and protocols enables efficient network setup, security management, and troubleshooting, ensuring seamless communication across different systems and platforms.

