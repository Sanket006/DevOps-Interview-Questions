# Networking Interview Questions and Answers (DevOps-Focused)

---

## 1) What is the OSI model? Explain all 7 layers.

**Answer:**
The OSI (Open Systems Interconnection) model is a conceptual framework used to understand network communication. It has 7 layers:

1. Physical – Deals with cables, bits, electrical signals.
2. Data Link – MAC address, switches, frames.
3. Network – IP addressing, routing (routers).
4. Transport – TCP/UDP, ports, reliability.
5. Session – Session establishment and management.
6. Presentation – Encryption, compression, formatting.
7. Application – HTTP, FTP, DNS, SMTP.

---

## 2) What is the difference between TCP and UDP?

**Answer:**
TCP (Transmission Control Protocol):

* Connection-oriented
* Reliable (acknowledgment based)
* Slower but accurate
* Used in HTTP, HTTPS, SSH

UDP (User Datagram Protocol):

* Connectionless
* Faster
* No guarantee of delivery
* Used in DNS, streaming, VoIP

---

## 3) What is the difference between Public IP and Private IP?

**Answer:**
Public IP:

* Globally accessible
* Assigned by ISP

Private IP:

* Used inside internal networks
* Not accessible from the internet
* Example ranges:

  * 10.0.0.0/8
  * 172.16.0.0 – 172.31.255.255
  * 192.168.0.0/16

---

## 4) What is DNS and how does it work?

**Answer:**
DNS (Domain Name System) converts domain names into IP addresses.

Steps:

1. User types google.com
2. Browser checks cache
3. Query goes to DNS resolver
4. Resolver contacts root server
5. Then TLD server (.com)
6. Then authoritative server
7. Returns IP address

---

## 5) What is a subnet?

**Answer:**
Subnetting is dividing a large network into smaller networks.

Example:
192.168.1.0/24

* 255 IPs available

Subnet mask determines how many hosts can exist.

---

## 6) What is NAT?

**Answer:**
NAT (Network Address Translation) allows multiple private IP addresses to use one public IP address.

Used in:

* Home routers
* Cloud VPC architectures

---

## 7) What is a Port number?

**Answer:**
Port identifies a service running on a machine.

Examples:

* 80 → HTTP
* 443 → HTTPS
* 22 → SSH
* 3306 → MySQL

---

## 8) What is CIDR?

**Answer:**
CIDR (Classless Inter-Domain Routing) is a method to allocate IP addresses using prefix notation.

Example:
192.168.1.0/24

/24 means first 24 bits are network bits.

---

## 9) What is a Firewall?

**Answer:**
A firewall filters incoming and outgoing traffic based on rules.

In Cloud:

* AWS Security Groups
* NACLs

---

## 10) What is a Load Balancer?

**Answer:**
A load balancer distributes traffic across multiple servers to improve availability and scalability.

Types:

* Layer 4 (TCP based)
* Layer 7 (HTTP based)

---

## 11) What happens when you type a URL in the browser?

**Answer:**

1. DNS resolution
2. TCP handshake
3. TLS handshake (if HTTPS)
4. HTTP request sent
5. Server response
6. Browser renders page

---

## 12) What is ARP?

**Answer:**
ARP (Address Resolution Protocol) maps IP address to MAC address in local network.

---

## 13) What is the difference between Switch and Router?

**Answer:**
Switch:

* Works at Layer 2
* Uses MAC address

Router:

* Works at Layer 3
* Uses IP address

---

## 14) What is Latency, Bandwidth, and Throughput?

**Answer:**
Latency – Delay in communication.
Bandwidth – Maximum data capacity.
Throughput – Actual data transferred.

---

## 15) What is a VPC?

**Answer:**
VPC (Virtual Private Cloud) is a logically isolated network in cloud.

In AWS:

* Subnets
* Route tables
* Internet Gateway
* NAT Gateway
* Security Groups

---

# 🔥 Advanced DevOps-Level Networking Questions

## 16) Explain 3-way TCP handshake.

SYN → SYN-ACK → ACK

The TCP three-way handshake is a process used to establish a reliable connection between two devices in a TCP/IP network. 
It involves three steps:

* SYN: The client sends a SYN (synchronize) packet to the server to initiate a connection.

* SYN-ACK: The server responds with a SYN-ACK (synchronize-acknowledge) packet, acknowledging the receipt of the SYN packet and indicating its willingness to establish a connection.

* ACK: The client sends an ACK (acknowledge) packet back to the server, confirming the connection establishment.

This handshake ensures that both parties are ready for communication and have synchronized their sequence numbers. 

## 17) What is difference between Security Group and NACL?

Security Group:

* Stateful
* Instance level

NACL:

* Stateless
* Subnet level

## 18) How does Kubernetes networking work?

* Every pod gets unique IP
* Uses CNI plugin
* kube-proxy manages service routing

## 19) What is HTTPS internally?

HTTP + TLS encryption.

## 20) How to debug networking issue in production?

Steps:

* ping
* traceroute
* nslookup
* netstat
* curl
* check firewall rules

---