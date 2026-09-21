# Computer Networks — Infosys Interview Preparation

## 🗺️ Question Roadmap

**Basics**
1. 🔴 What is a Computer Network? OSI Model overview
2. 🔴 TCP vs UDP
3. 🔴 HTTP vs HTTPS
4. 🔴 Router vs Switch vs Hub   
5. 🟠 What is an IP Address? IPv4 vs IPv6   

**Core Concepts**
6. 🔴 What happens when you type a URL in a browser? (classic question)
7. 🟠 DNS — what it is and how it works    
8. 🟠 TCP 3-Way Handshake    
9. 🟠 What is a Port? Well-known ports

**Intermediate/Advanced**
10. 🟡 What is DHCP?
11. 🟡 What is NAT?
12. 🟡 What is a Firewall?
13. 🟡 REST API and Computer Networks connection (HTTP methods, status codes)

---

## A. 🔴 MUST KNOW

### Q1. What is a Computer Network? OSI Model overview

**Answer:**
> "A computer network is a collection of interconnected devices that can communicate and share resources like data, files, and internet access."

**OSI Model (7 layers) — explain top-down or bottom-up, know the order:**

| Layer | Name | Example/Purpose |
|---|---|---|
| 7 | Application | HTTP, FTP, DNS — user-facing protocols |
| 6 | Presentation | Encryption, compression, data translation |
| 5 | Session | Manages sessions/connections between apps |
| 4 | Transport | TCP/UDP — reliable delivery, segmentation |
| 3 | Network | IP, routing — logical addressing |
| 2 | Data Link | MAC addresses, switches, frames |
| 1 | Physical | Cables, signals, hardware transmission |

**Memory trick:** "**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing" (top to bottom: Application → Physical).

**Interview Tip:** Don't just recite the list — mention that TCP/IP model (used in practice) has only 4 layers (Application, Transport, Internet, Network Access), and OSI is more of a theoretical/teaching reference model.
**Follow-up:** Which layer does a router operate at? (Layer 3 — Network.) Which layer does a switch operate at? (Layer 2 — Data Link.)

---

### Q2. TCP vs UDP

| Aspect | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (3-way handshake) | Connectionless |
| Reliability | Reliable — guarantees delivery, in-order | Unreliable — no guarantee |
| Speed | Slower (overhead of acknowledgments, retransmission) | Faster (no overhead) |
| Use cases | Web browsing (HTTP), email, file transfer | Video streaming, online gaming, DNS, VoIP |
| Ordering | Maintains order of packets | No ordering guarantee |

**Interview Tip:** Say: "TCP prioritizes reliability over speed — it's used when data integrity matters, like loading a webpage or sending an email. UDP prioritizes speed over reliability — it's used when occasional data loss is acceptable, like a live video call, because retransmitting old frames is pointless."
**Common mistake:** Saying UDP is "always bad" — it's a deliberate design trade-off, not a flaw.
**Follow-up:** Why does DNS use UDP? (Small, quick request/response — doesn't need TCP's overhead. Falls back to TCP for larger responses like zone transfers.)

---

### Q3. HTTP vs HTTPS

| Aspect | HTTP | HTTPS |
|---|---|---|
| Security | No encryption — plain text | Encrypted using SSL/TLS |
| Port | 80 | 443 |
| Data safety | Vulnerable to eavesdropping/man-in-the-middle attacks | Protects data confidentiality and integrity |
| Certificate | Not needed | Requires an SSL/TLS certificate |

**Interview Tip:** Mention briefly what SSL/TLS does: it encrypts data between client and server and verifies the server's identity via a certificate authority. Freshers don't need deep cryptography detail — a confident high-level answer is enough.
**Follow-up:** What is SSL handshake (briefly)? (Client and server exchange certificates/keys to establish an encrypted session before actual data transfer begins.)

---

### Q4. Router vs Switch vs Hub

| Device | Layer | Function |
|---|---|---|
| Hub | Physical (Layer 1) | Broadcasts data to all connected devices — no intelligence |
| Switch | Data Link (Layer 2) | Forwards data only to the intended device using MAC addresses |
| Router | Network (Layer 3) | Connects different networks, forwards data using IP addresses |

**Interview Tip:** Say: "A hub just broadcasts everything to every device — inefficient and outdated. A switch is smarter — it learns MAC addresses and sends data only to the right device on the same network. A router connects entirely different networks together, like your home network to the internet."
**Follow-up:** How does a switch learn MAC addresses? (It builds a MAC address table by observing the source address of incoming frames.)

---

### Q5. What is an IP Address? IPv4 vs IPv6

> "An IP address is a unique numerical identifier assigned to each device on a network, used for locating and communicating with that device."

| Aspect | IPv4 | IPv6 |
|---|---|---|
| Length | 32-bit | 128-bit |
| Format | Decimal, dotted (e.g., 192.168.1.1) | Hexadecimal, colon-separated |
| Address space | ~4.3 billion addresses | Practically unlimited |
| Reason for IPv6 | IPv4 addresses are running out | Solves address exhaustion |

**Interview Tip:** Mention **Public vs Private IP** as a likely follow-up: private IPs (like 192.168.x.x) are used within local networks and translated to a public IP via NAT for internet access.
**Follow-up:** What is a subnet mask? (High-level: it separates the network portion from the host portion of an IP address.)

---

## B. 🟠 VERY IMPORTANT

### Q6. What Happens When You Type a URL in a Browser? (Classic Question)

This is a **very common Infosys/any-company interview question** that tests end-to-end networking understanding. Structure your answer step by step:

1. **DNS Resolution** — Browser checks cache, then queries DNS to resolve the domain name to an IP address.
2. **TCP Connection** — Browser establishes a TCP connection with the server via the 3-way handshake.
3. **TLS Handshake** (if HTTPS) — Encryption keys are exchanged.
4. **HTTP Request** — Browser sends an HTTP GET request for the page.
5. **Server Processing** — Server processes the request (may hit a database, application server, etc.) and sends back an HTTP response.
6. **Rendering** — Browser parses HTML/CSS/JS and renders the page, making additional requests for images, scripts, etc.

**Interview Tip:** This question tests whether you can connect DNS + TCP + HTTP + browser rendering into one coherent story. Practice saying this out loud in under a minute — it's a very commonly repeated interview question.
**Follow-up:** What is DNS caching? What if the server is down — what error does the browser show?

---

### Q7. DNS (Domain Name System)

> "DNS translates human-readable domain names (like google.com) into IP addresses that computers use to identify each other on the network. It works like the internet's phonebook."

**Interview Tip:** Mention it's a **hierarchical, distributed system** — Root servers → TLD servers (.com, .org) → Authoritative servers, with caching at multiple levels (browser, OS, ISP) to speed up repeated lookups.
**Follow-up:** What is the difference between DNS A record and CNAME record? (A record maps a domain to an IP; CNAME maps a domain to another domain name — good to know at a basic level, not mandatory.)

---

### Q8. TCP 3-Way Handshake

**Explain the concept first:** Before any data is sent over TCP, the client and server must establish a reliable connection.

**Steps:**
1. **SYN** — Client sends a SYN (synchronize) packet to the server to initiate connection.
2. **SYN-ACK** — Server responds with SYN-ACK, acknowledging and agreeing to connect.
3. **ACK** — Client sends ACK back, connection is now established.

```
Client  --- SYN --->  Server
Client  <-- SYN-ACK -- Server
Client  --- ACK --->  Server
    (connection established)
```

**Interview Tip:** Also briefly mention the **4-way termination** (FIN, ACK, FIN, ACK) to close a connection if asked as a follow-up.
**Follow-up:** Why 3 steps and not 2? (Both sides need to confirm they can send AND receive — 2 steps only confirm one direction.)

---

### Q9. What is a Port? Well-Known Ports

> "A port is a logical endpoint that identifies a specific process/service running on a device, allowing multiple applications to use the network simultaneously on the same IP address."

| Port | Service |
|---|---|
| 80 | HTTP |
| 443 | HTTPS |
| 21 | FTP |
| 22 | SSH |
| 25 | SMTP (email) |
| 53 | DNS |
| 3306 | MySQL |

**Interview Tip:** Say: "IP address identifies the device, port identifies the specific application/service on that device." A good, quick, memorable line.

---

## C. 🟡 GOOD TO KNOW

### Q10. DHCP

> "DHCP (Dynamic Host Configuration Protocol) automatically assigns IP addresses to devices on a network, so they don't need to be manually configured."

**Interview Tip:** Mention it also assigns subnet mask, default gateway, and DNS server info — not just the IP.

---

### Q11. NAT (Network Address Translation)

> "NAT translates private IP addresses used within a local network into a single public IP address for communication over the internet, and vice versa for incoming traffic."

**Interview Tip:** Tie this back to Q5 — explain why NAT is needed (IPv4 address scarcity, and it also adds a layer of security by hiding internal IPs).

---

### Q12. Firewall

> "A firewall monitors and controls incoming and outgoing network traffic based on predefined security rules, acting as a barrier between a trusted internal network and untrusted external networks."

**Interview Tip:** Mention it can be hardware or software-based, and works by allowing/blocking traffic based on IP, port, or protocol rules.

---

### Q13. REST API and Networking Connection

Since REST APIs run over HTTP, interviewers sometimes blend CN and REST API questions:

| HTTP Method | Purpose |
|---|---|
| GET | Retrieve data |
| POST | Create new data |
| PUT | Update/replace data |
| PATCH | Partially update data |
| DELETE | Remove data |

**Common status codes:** 200 (OK), 201 (Created), 400 (Bad Request), 401 (Unauthorized), 404 (Not Found), 500 (Internal Server Error).

**Interview Tip:** If REST API is on your resume, be ready to connect it here: "A REST API call is fundamentally an HTTP request — it goes through DNS resolution, TCP handshake, and then the actual HTTP GET/POST exchange."

---

## ⭐ Top Questions to Memorize

1. TCP vs UDP
2. HTTP vs HTTPS
3. Router vs Switch vs Hub
4. "What happens when you type a URL in the browser" — full flow
5. OSI model layers, in order, with one example per layer
6. TCP 3-way handshake steps
7. DNS — what it is and why it's needed
8. IPv4 vs IPv6
9. Port — what it is, common port numbers (80, 443, 22)
10. NAT — why it's needed
11. Public vs Private IP
12. HTTP methods and status codes (if REST is on resume)

## 🎯 Interview Preparation Checklist

- [ ] Can explain "what happens when you type a URL" fluently in under a minute
- [ ] Can list OSI layers top-to-bottom or bottom-to-top confidently
- [ ] Can explain TCP vs UDP with a clear use-case example for each
- [ ] Can draw/describe the TCP 3-way handshake
- [ ] Can explain router vs switch vs hub with the "layer" each operates at
- [ ] Know common port numbers: 80, 443, 22, 21, 53
- [ ] Can connect REST API request flow to underlying HTTP/TCP/DNS concepts
