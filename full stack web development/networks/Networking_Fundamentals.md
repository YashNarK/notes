# Networking Fundamentals — Deep Dive

---

## Table of Contents

- [OSI Model](#osi-model)
- [TCP/IP Model](#tcpip-model)
- [IP Addressing](#ip-addressing)
- [Subnetting](#subnetting)
- [DNS (Domain Name System)](#dns-domain-name-system)
- [TCP vs UDP](#tcp-vs-udp)
- [TCP Three-Way Handshake](#tcp-three-way-handshake)
- [HTTP / HTTPS](#http--https)
- [HTTP/1.1 vs HTTP/2 vs HTTP/3](#http11-vs-http2-vs-http3)
- [TLS / SSL](#tls--ssl)
- [Ports and Protocols](#ports-and-protocols)
- [NAT (Network Address Translation)](#nat-network-address-translation)
- [Firewalls](#firewalls)
- [Load Balancers](#load-balancers)
- [Proxy vs Reverse Proxy](#proxy-vs-reverse-proxy)
- [WebSockets](#websockets)
- [CORS (Cross-Origin Resource Sharing)](#cors-cross-origin-resource-sharing)
- [CDN (Content Delivery Network)](#cdn-content-delivery-network)
- [Common Network Debugging Tools](#common-network-debugging-tools)

---

# OSI Model

The OSI (Open Systems Interconnection) model is a conceptual framework that standardizes network communication into **7 layers**. Each layer has a specific role and communicates with the layers directly above and below it.

| Layer | Name | Function | Protocols / Examples | Data Unit |
|---|---|---|---|---|
| **7** | Application | User-facing services (HTTP, email, file transfer) | HTTP, FTP, SMTP, DNS, SSH | Data |
| **6** | Presentation | Data formatting, encryption, compression | SSL/TLS, JPEG, ASCII, JSON | Data |
| **5** | Session | Establishes, manages, and terminates sessions | NetBIOS, RPC, PPTP | Data |
| **4** | Transport | Reliable (TCP) or fast (UDP) end-to-end delivery | TCP, UDP | Segment / Datagram |
| **3** | Network | Logical addressing and routing between networks | IP, ICMP, ARP, OSPF | Packet |
| **2** | Data Link | Physical addressing (MAC), error detection, frames | Ethernet, Wi-Fi (802.11), PPP | Frame |
| **1** | Physical | Raw bit transmission over physical medium | Cables, hubs, radio signals | Bits |

**Mnemonic** (top to bottom): **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing

### How Data Flows Through the OSI Model

**Sending** (top → bottom): Each layer adds its own header (encapsulation).
```
Application Data → [Transport Header + Data] → [Network Header + Segment] → [Data Link Header + Packet + Trailer] → Bits on wire
```

**Receiving** (bottom → top): Each layer strips its header (decapsulation) and passes data up.

---

# TCP/IP Model

The TCP/IP model is the practical, real-world model the internet actually uses. It condenses OSI's 7 layers into **4 layers**.

| TCP/IP Layer | OSI Equivalent | Function | Protocols |
|---|---|---|---|
| **Application** | Layers 5, 6, 7 | Application-level protocols | HTTP, DNS, FTP, SMTP, SSH |
| **Transport** | Layer 4 | End-to-end communication | TCP, UDP |
| **Internet** | Layer 3 | Addressing and routing | IP, ICMP, ARP |
| **Network Access** | Layers 1, 2 | Physical network transmission | Ethernet, Wi-Fi |

### OSI vs TCP/IP

| Aspect | OSI | TCP/IP |
|---|---|---|
| Layers | 7 | 4 |
| Nature | Theoretical / reference model | Practical / implementation model |
| Developed by | ISO | DARPA / DoD |
| Session & Presentation | Separate layers | Merged into Application |
| Usage | Teaching and design | Actual internet communication |

---

# IP Addressing

## IPv4

- **32-bit** address, written as 4 octets in decimal: `192.168.1.100`
- Total addresses: ~4.3 billion (2³²)
- Divided into **network** and **host** portions based on subnet mask

### IPv4 Address Classes

| Class | Range | Default Subnet Mask | Purpose |
|---|---|---|---|
| A | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 (/8) | Large networks |
| B | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 (/16) | Medium networks |
| C | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 (/24) | Small networks |
| D | 224.0.0.0 – 239.255.255.255 | — | Multicast |
| E | 240.0.0.0 – 255.255.255.255 | — | Experimental |

### Private IP Ranges (RFC 1918) — not routable on the public internet

| Class | Range | CIDR |
|---|---|---|
| A | 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| B | 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| C | 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

### Special Addresses

| Address | Purpose |
|---|---|
| `127.0.0.1` | Loopback (localhost) |
| `0.0.0.0` | All interfaces / unspecified |
| `255.255.255.255` | Broadcast |
| `169.254.x.x` | Link-local (APIPA — when DHCP fails) |

## IPv6

- **128-bit** address, written in 8 groups of 4 hex digits: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Total addresses: 2¹²⁸ (practically unlimited)
- No need for NAT — every device can have a globally unique address
- Leading zeros can be dropped: `2001:db8:85a3::8a2e:370:7334`

---

# Subnetting

Subnetting divides a large network into smaller, more manageable **subnets**.

## CIDR Notation

CIDR (Classless Inter-Domain Routing) uses a `/` followed by the number of network bits.

| CIDR | Subnet Mask | Usable Hosts | Notes |
|---|---|---|---|
| /8 | 255.0.0.0 | 16,777,214 | Class A |
| /16 | 255.255.0.0 | 65,534 | Class B |
| /24 | 255.255.255.0 | 254 | Class C |
| /25 | 255.255.255.128 | 126 | Half a /24 |
| /26 | 255.255.255.192 | 62 | Quarter of a /24 |
| /27 | 255.255.255.224 | 30 | — |
| /28 | 255.255.255.240 | 14 | — |
| /30 | 255.255.255.252 | 2 | Point-to-point links |
| /32 | 255.255.255.255 | 1 | Single host |

**Formula**: Usable hosts = 2^(32 - prefix) - 2 (subtract network and broadcast addresses)

### Subnetting Example

Given: `192.168.10.0/24` — split into 4 equal subnets.

Need 4 subnets → borrow 2 bits → /26 (each subnet has 62 usable hosts)

| Subnet | Network Address | Usable Range | Broadcast |
|---|---|---|---|
| 1 | 192.168.10.0 | 192.168.10.1 – .62 | 192.168.10.63 |
| 2 | 192.168.10.64 | 192.168.10.65 – .126 | 192.168.10.127 |
| 3 | 192.168.10.128 | 192.168.10.129 – .190 | 192.168.10.191 |
| 4 | 192.168.10.192 | 192.168.10.193 – .254 | 192.168.10.255 |

---

# DNS (Domain Name System)

DNS translates human-readable domain names (`google.com`) into IP addresses (`142.250.80.46`).

## DNS Resolution Flow

```
Browser Cache → OS Cache → Recursive Resolver (ISP) → Root Server → TLD Server → Authoritative Server → IP Address
```

1. **Browser cache**: Already visited? Use cached IP.
2. **OS cache**: Check `/etc/hosts` or system DNS cache.
3. **Recursive resolver**: ISP's DNS server looks it up on your behalf.
4. **Root server**: Directs to the correct TLD server (`.com`, `.org`, `.io`).
5. **TLD server**: Directs to the authoritative nameserver for the domain.
6. **Authoritative server**: Returns the actual IP address.

## DNS Record Types

| Record | Purpose | Example |
|---|---|---|
| **A** | Maps domain to IPv4 address | `example.com → 93.184.216.34` |
| **AAAA** | Maps domain to IPv6 address | `example.com → 2606:2800:220:1:...` |
| **CNAME** | Alias for another domain | `www.example.com → example.com` |
| **MX** | Mail exchange server | `example.com → mail.example.com` |
| **NS** | Nameserver for the domain | `example.com → ns1.dnshost.com` |
| **TXT** | Arbitrary text (SPF, DKIM, verification) | `v=spf1 include:_spf.google.com` |
| **SRV** | Service location (host + port) | Used by SIP, XMPP |
| **PTR** | Reverse DNS (IP → domain) | `34.216.184.93 → example.com` |
| **SOA** | Start of authority (zone metadata) | TTL, admin email, serial number |

## DNS TTL (Time to Live)

- Specifies how long a DNS record is cached (in seconds).
- Low TTL (300s): Changes propagate fast, but more DNS lookups.
- High TTL (86400s): Fewer lookups, but changes take longer to propagate.

---

# TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| **Connection** | Connection-oriented (handshake) | Connectionless |
| **Reliability** | Guaranteed delivery, ordering, retransmission | No guarantees — best-effort |
| **Speed** | Slower (overhead from reliability) | Faster (minimal overhead) |
| **Flow Control** | Yes (windowing) | No |
| **Error Checking** | Yes + retransmission | Checksum only |
| **Ordering** | Guaranteed in-order delivery | No ordering |
| **Use Cases** | HTTP, HTTPS, FTP, SSH, email | DNS, video streaming, gaming, VoIP |
| **Header Size** | 20 bytes | 8 bytes |

**When to use TCP**: When data integrity matters (web pages, file transfers, APIs).
**When to use UDP**: When speed matters more than reliability (live video, gaming, DNS queries).

---

# TCP Three-Way Handshake

TCP establishes a connection using a **three-way handshake** before any data is sent:

```
Client                     Server
  |                          |
  |--- SYN (seq=x) -------->|   1. Client: "I want to connect" (sends SYN with sequence number)
  |                          |
  |<-- SYN-ACK (seq=y, ------|   2. Server: "OK, I acknowledge" (sends SYN + ACK)
  |    ack=x+1)              |
  |                          |
  |--- ACK (ack=y+1) ------>|   3. Client: "Got it, let's go" (sends ACK)
  |                          |
  |===== DATA TRANSFER ======|
```

### Connection Termination (Four-Way Handshake)

```
Client                     Server
  |--- FIN ----------------->|   1. Client: "I'm done sending"
  |<-- ACK -----------------|   2. Server: "Acknowledged"
  |<-- FIN -----------------|   3. Server: "I'm done sending too"
  |--- ACK ----------------->|   4. Client: "Acknowledged — connection closed"
```

---

# HTTP / HTTPS

## HTTP (HyperText Transfer Protocol)

- **Application-layer** protocol for transmitting hypermedia documents.
- Operates over **TCP** (port 80 by default).
- **Stateless** — each request is independent; no memory of previous requests.

### HTTP Request Structure

```
GET /api/users HTTP/1.1          ← Request line (method, path, version)
Host: example.com                ← Headers
Authorization: Bearer token123
Content-Type: application/json

{ "name": "John" }              ← Body (for POST, PUT, PATCH)
```

### HTTP Response Structure

```
HTTP/1.1 200 OK                 ← Status line
Content-Type: application/json
Content-Length: 42

{ "id": 1, "name": "John" }    ← Response body
```

### Common HTTP Methods

| Method | Purpose | Idempotent | Safe |
|---|---|---|---|
| GET | Retrieve a resource | ✅ Yes | ✅ Yes |
| POST | Create a resource | ❌ No | ❌ No |
| PUT | Replace a resource entirely | ✅ Yes | ❌ No |
| PATCH | Partially update a resource | ⚠️ Depends | ❌ No |
| DELETE | Remove a resource | ✅ Yes | ❌ No |
| HEAD | Like GET but no body | ✅ Yes | ✅ Yes |
| OPTIONS | Discover allowed methods (CORS preflight) | ✅ Yes | ✅ Yes |

## HTTPS

- HTTP over **TLS/SSL** — encrypted communication.
- Default port: **443**.
- Provides **confidentiality**, **integrity**, and **authentication**.
- Required for any sensitive data (passwords, payments, personal info).

---

# HTTP/1.1 vs HTTP/2 vs HTTP/3

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| **Year** | 1997 | 2015 | 2022 |
| **Transport** | TCP | TCP | **QUIC (over UDP)** |
| **Multiplexing** | ❌ No (one request per connection) | ✅ Yes (multiple streams per connection) | ✅ Yes |
| **Head-of-line Blocking** | ✅ Yes (at TCP level) | ⚠️ Partially (at TCP level) | ❌ No (resolved by QUIC) |
| **Header Compression** | ❌ No | ✅ HPACK | ✅ QPACK |
| **Server Push** | ❌ No | ✅ Yes | ✅ Yes |
| **Connection Setup** | TCP handshake + TLS handshake | Same as 1.1 | **0-RTT or 1-RTT** (faster) |
| **Binary Protocol** | ❌ Text-based | ✅ Binary | ✅ Binary |

### Key Takeaway

- **HTTP/2**: Multiplexing eliminates the need for workarounds like domain sharding and sprite sheets.
- **HTTP/3**: Uses QUIC (UDP-based) to eliminate TCP-level head-of-line blocking and enable faster connection setup.

---

# TLS / SSL

## What is TLS?

TLS (Transport Layer Security) is the successor to SSL. It encrypts communication between client and server.

- **SSL** (Secure Sockets Layer): Deprecated (SSL 2.0, 3.0).
- **TLS** 1.2 and **TLS 1.3** are the current standards.

## TLS Handshake (TLS 1.2 — Simplified)

```
Client                           Server
  |--- ClientHello -------------->|   Supported cipher suites, TLS version
  |<-- ServerHello + Certificate -|   Chosen cipher, server's public certificate
  |--- Key Exchange + Finished -->|   Client generates pre-master secret, encrypted with server's public key
  |<-- Finished ------------------|   Server decrypts, both derive session keys
  |===== Encrypted Session =======|   All data encrypted with symmetric session keys
```

## TLS 1.3 Improvements

- Handshake in **1 round trip** (1-RTT) instead of 2.
- Supports **0-RTT** resumption for returning clients.
- Removed insecure algorithms (RSA key exchange, CBC ciphers).
- Only supports **forward secrecy** (ephemeral keys).

---

# Ports and Protocols

| Port | Protocol | Description |
|---|---|---|
| 20, 21 | FTP | File Transfer Protocol (data / control) |
| 22 | SSH | Secure Shell (remote access) |
| 23 | Telnet | Unencrypted remote access (insecure) |
| 25 | SMTP | Simple Mail Transfer Protocol |
| 53 | DNS | Domain Name System |
| 80 | HTTP | Web traffic (unencrypted) |
| 110 | POP3 | Post Office Protocol (email retrieval) |
| 143 | IMAP | Internet Message Access Protocol (email) |
| 443 | HTTPS | Web traffic (encrypted) |
| 3000 | — | Common dev server port (Express, React) |
| 3306 | MySQL | MySQL database |
| 5432 | PostgreSQL | PostgreSQL database |
| 6379 | Redis | Redis in-memory store |
| 27017 | MongoDB | MongoDB database |

- Ports **0–1023**: Well-known (system) ports — require root/admin
- Ports **1024–49151**: Registered ports
- Ports **49152–65535**: Dynamic/ephemeral ports (used by clients)

---

# NAT (Network Address Translation)

NAT allows multiple devices on a private network to share a single public IP address.

```
[Device 1: 192.168.1.10] ─┐
[Device 2: 192.168.1.11] ─┤── Router (NAT) ── Public IP: 203.0.113.5 ── Internet
[Device 3: 192.168.1.12] ─┘
```

### Types of NAT

| Type | Description |
|---|---|
| **Static NAT** | One-to-one mapping (private IP ↔ public IP) |
| **Dynamic NAT** | Maps private IPs to a pool of public IPs |
| **PAT (Port Address Translation)** | Many-to-one — uses port numbers to distinguish connections. This is what home routers use. |

### Why NAT Exists

- **IPv4 address exhaustion**: Not enough public IPs for every device.
- **Security**: Hides internal network structure from the internet.
- **Will be less needed with IPv6** (every device gets a unique address).

---

# Firewalls

A firewall monitors and controls network traffic based on predefined security rules.

## Types of Firewalls

| Type | Description |
|---|---|
| **Packet Filter** | Inspects individual packets (IP, port, protocol). Stateless. |
| **Stateful Inspection** | Tracks connection state — allows return traffic for established connections. |
| **Application Layer (WAF)** | Inspects HTTP content — blocks SQL injection, XSS, etc. |
| **Next-Gen Firewall (NGFW)** | Combines stateful inspection + deep packet inspection + application awareness. |

## Common Firewall Rules

```
Allow TCP port 443 inbound (HTTPS)
Allow TCP port 22 inbound from 10.0.0.0/8 (SSH from internal only)
Deny all inbound by default
Allow all outbound
```

---

# Load Balancers

A load balancer distributes incoming traffic across multiple servers to ensure no single server is overwhelmed.

## Load Balancing Algorithms

| Algorithm | Description |
|---|---|
| **Round Robin** | Requests distributed sequentially across servers |
| **Weighted Round Robin** | Like round robin, but servers with more capacity get more traffic |
| **Least Connections** | Routes to the server handling the fewest active connections |
| **IP Hash** | Client IP determines which server handles the request (sticky sessions) |
| **Random** | Randomly selects a server |

## Layer 4 vs Layer 7 Load Balancing

| Aspect | Layer 4 (Transport) | Layer 7 (Application) |
|---|---|---|
| Operates on | TCP/UDP (IP + port) | HTTP content (URL, headers, cookies) |
| Speed | Faster (less inspection) | Slower (inspects content) |
| Intelligence | Dumb routing | Smart routing (path-based, content-based) |
| Example | AWS NLB, HAProxy TCP mode | AWS ALB, Nginx, HAProxy HTTP mode |

### Layer 7 Routing Example (Nginx)

```nginx
upstream api_servers {
    server 10.0.1.1:3000;
    server 10.0.1.2:3000;
}

upstream static_servers {
    server 10.0.2.1:80;
}

server {
    listen 80;

    location /api/ {
        proxy_pass http://api_servers;
    }

    location /static/ {
        proxy_pass http://static_servers;
    }
}
```

---

# Proxy vs Reverse Proxy

## Forward Proxy

Sits between **clients** and the internet. The client sends requests through the proxy.

```
[Client] → [Forward Proxy] → [Internet / Server]
```

**Use cases**: Anonymity, content filtering, geo-unblocking, corporate access control.

## Reverse Proxy

Sits between the **internet** and backend servers. External clients don't know about backend servers.

```
[Internet / Client] → [Reverse Proxy] → [Backend Server 1, 2, 3...]
```

**Use cases**: Load balancing, SSL termination, caching, security (hide backend IPs), compression.

**Examples**: Nginx, HAProxy, AWS ALB/CloudFront, Cloudflare.

| Aspect | Forward Proxy | Reverse Proxy |
|---|---|---|
| Protects | Clients | Servers |
| Client knows about it? | Yes (configured in browser/OS) | No (transparent) |
| Server knows about it? | No (sees proxy IP) | Yes (configured to accept from proxy) |

---

# WebSockets

WebSocket provides **full-duplex, persistent** communication over a single TCP connection.

## HTTP vs WebSocket

| Aspect | HTTP | WebSocket |
|---|---|---|
| Communication | Request-response (half-duplex) | Full-duplex (bidirectional) |
| Connection | New connection per request (or keep-alive) | Single persistent connection |
| Overhead | Headers sent with every request | Minimal frame overhead after handshake |
| Initiated by | Client only | Client or server can send at any time |
| Protocol | `http://` / `https://` | `ws://` / `wss://` |

## WebSocket Handshake

WebSocket starts as an HTTP request and **upgrades**:

```
Client → Server:
GET /chat HTTP/1.1
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==

Server → Client:
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

After the 101 response, both sides communicate via WebSocket frames (not HTTP).

**Use cases**: Chat apps, real-time dashboards, live notifications, multiplayer games, collaborative editing.

---

# CORS (Cross-Origin Resource Sharing)

CORS is a browser security mechanism that **restricts cross-origin HTTP requests**. By default, browsers block requests from one origin (`http://localhost:3000`) to a different origin (`http://api.example.com`).

## How CORS Works

1. **Simple requests** (GET, POST with standard headers): Browser sends request with `Origin` header; server responds with `Access-Control-Allow-Origin`.
2. **Preflight requests** (PUT, DELETE, custom headers): Browser sends an `OPTIONS` request first to check if the actual request is allowed.

### Preflight Flow

```
Browser → Server:  OPTIONS /api/users
                   Origin: http://localhost:3000
                   Access-Control-Request-Method: DELETE
                   Access-Control-Request-Headers: Authorization

Server → Browser:  Access-Control-Allow-Origin: http://localhost:3000
                   Access-Control-Allow-Methods: GET, POST, PUT, DELETE
                   Access-Control-Allow-Headers: Authorization
                   Access-Control-Max-Age: 86400

Browser → Server:  DELETE /api/users/123   (actual request)
```

### Express CORS Configuration

```javascript
const cors = require('cors');

// Allow specific origin
app.use(cors({
  origin: 'https://myapp.com',
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true   // Allow cookies
}));
```

---

# CDN (Content Delivery Network)

A CDN is a geographically distributed network of servers that caches and delivers content from the server closest to the user.

## How a CDN Works

```
User (India) → CDN Edge Server (Mumbai) → [Cache HIT? Return content : Fetch from Origin Server (US)]
```

1. User requests a resource (image, CSS, JS).
2. CDN edge server closest to user checks its cache.
3. **Cache HIT**: Returns cached content instantly.
4. **Cache MISS**: Fetches from origin server, caches it, returns to user.

## Benefits

| Benefit | Description |
|---|---|
| **Reduced Latency** | Content served from nearby edge servers |
| **Lower Server Load** | Origin server handles fewer requests |
| **DDoS Protection** | CDN absorbs large traffic spikes |
| **High Availability** | If one edge goes down, traffic routes to another |
| **Bandwidth Savings** | Cached content reduces data transfer from origin |

## Popular CDN Providers

- **Cloudflare** — Free tier, WAF, DDoS protection, DNS
- **AWS CloudFront** — Integrates with S3, Lambda@Edge
- **Akamai** — Enterprise-grade, largest network
- **Fastly** — Edge computing, real-time purging
- **Vercel / Netlify Edge** — Built-in CDN for frontend deployments

---

# Common Network Debugging Tools

| Tool | Purpose | Example |
|---|---|---|
| `ping` | Test if a host is reachable (ICMP) | `ping google.com` |
| `traceroute` / `tracert` | Show the path packets take to a host | `traceroute google.com` |
| `nslookup` | Query DNS records | `nslookup example.com` |
| `dig` | Advanced DNS lookup | `dig example.com A` |
| `curl` | Send HTTP requests from terminal | `curl -v https://api.example.com` |
| `wget` | Download files from the web | `wget https://example.com/file.zip` |
| `netstat` / `ss` | Show active connections and listening ports | `netstat -tlnp` |
| `ifconfig` / `ip addr` | Show network interface configuration | `ip addr show` |
| `tcpdump` | Capture and analyze network packets | `tcpdump -i eth0 port 80` |
| `Wireshark` | GUI packet analyzer | Visual deep packet inspection |
| `telnet` | Test if a port is open | `telnet example.com 443` |
| `nc` (netcat) | Network Swiss army knife | `nc -zv example.com 80` |

### Quick Debugging Workflow

```bash
# 1. Can I reach the host?
ping api.example.com

# 2. Is DNS resolving correctly?
nslookup api.example.com

# 3. What path do packets take?
traceroute api.example.com

# 4. Is the port open?
nc -zv api.example.com 443

# 5. What does the HTTP response look like?
curl -v https://api.example.com/health
```
