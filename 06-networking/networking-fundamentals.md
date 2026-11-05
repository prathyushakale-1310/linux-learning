# Networking Fundamentals for DevOps

## 1. What Is Computer Networking?

Computer networking is the process of connecting two or more devices so they can communicate and share data and resources.

Devices can include:
- Computers
- Servers
- Routers
- Switches
- Mobile phones
- Printers
- Cloud instances

**Example:** When you open a website, your computer communicates with a web server over a network and receives the requested webpage.

### Why Is Networking Important in DevOps?

Networking is important in DevOps because applications, servers, containers, and cloud services must communicate with each other.

DevOps engineers use networking to:
- Connect application servers and databases.
- Configure web servers such as Nginx.
- Troubleshoot application connectivity.
- Configure AWS VPCs, subnets, and security groups.
- Understand DNS and load balancing.
- Secure communication using HTTPS.
- Diagnose port and firewall issues.

---

## 2. Types of Networks

### 2.1 LAN – Local Area Network

A LAN connects devices within a small geographical area, such as a home, office, or building.

**Example:** Computers connected to the same office network.

### 2.2 WAN – Wide Area Network

A WAN connects networks over large geographical distances.

**Example:** The internet is the largest example of a WAN.

### 2.3 MAN – Metropolitan Area Network

A MAN connects multiple networks across a city or metropolitan area.

**Example:** A network connecting different branches of an organization within one city.

### 2.4 PAN – Personal Area Network

A PAN connects personal devices over a short distance.

**Example:** A mobile phone connected to wireless Bluetooth headphones.

### Interview Question: What is the difference between LAN and WAN?

**Answer:** LAN connects devices within a limited geographical area, such as an office, whereas WAN connects networks across large geographical areas, such as different cities or countries.

---

## 3. Network Devices

### 3.1 Router

A router connects different networks and forwards data packets between them using IP addresses.

**Example:** A home router connects your local network to the internet.

### 3.2 Switch

A switch connects multiple devices within the same local network. It forwards Ethernet frames using MAC addresses.

**Example:** An office switch connects multiple computers and printers.

### 3.3 Modem

A modem connects a network to an internet service provider by converting signals suitable for the communication medium.

### 3.4 Firewall

A firewall monitors and controls network traffic based on configured security rules.

It can allow or block traffic based on:
- Source and destination IP addresses.
- Port numbers.
- Protocols.
- Connection rules.

**DevOps example:** An AWS security group can allow inbound HTTPS traffic on port 443 while blocking unwanted inbound traffic.

### 3.5 Load Balancer

A load balancer distributes incoming requests across multiple servers.

**Example:** If three application servers are running, a load balancer can distribute requests among them to improve availability and performance.

---

## 4. OSI Model

The OSI (Open Systems Interconnection) model is a conceptual model that explains how data communication works through seven layers.

It helps us understand network communication and troubleshoot network problems.

### Seven Layers of the OSI Model

| Layer | Name | Main Function | Example |
|---|---|---|---|
| 7 | Application | Provides network services to applications | HTTP, DNS, SMTP |
| 6 | Presentation | Data formatting, encryption and compression | TLS-related presentation concepts, encoding |
| 5 | Session | Establishes and manages communication sessions | Session management |
| 4 | Transport | End-to-end communication and port numbers | TCP, UDP |
| 3 | Network | Logical addressing and routing | IP, routers |
| 2 | Data Link | Local network delivery using frames and MAC addresses | Ethernet, switches |
| 1 | Physical | Transmits raw bits through physical media | Cables, radio signals |

### Explanation of Each Layer

**Layer 1 – Physical Layer**

- Transmits raw bits over physical media.
- Deals with cables, connectors, radio signals, and electrical or optical signals.
- Example: Ethernet cable and fiber-optic cable.

**Layer 2 – Data Link Layer**

- Handles communication within a local network.
- Uses MAC addresses.
- Transfers data in the form of frames.
- Switches commonly operate at this layer.

**Layer 3 – Network Layer**

- Handles logical addressing and routing.
- Uses IP addresses.
- Transfers data in packets.
- Routers commonly operate at this layer.

**Layer 4 – Transport Layer**

- Provides communication between applications running on different devices.
- Uses port numbers.
- Main protocols are TCP and UDP.
- Data units are commonly called segments for TCP and datagrams for UDP.

**Layer 5 – Session Layer**

- Establishes, manages, and terminates communication sessions between applications.

**Layer 6 – Presentation Layer**

- Handles data representation, encoding, compression, and encryption-related functions in the OSI model.

**Layer 7 – Application Layer**

- Provides network services used by applications.
- Examples include HTTP, HTTPS, DNS, and SMTP.

### Easy Way to Remember the OSI Layers

From Layer 7 to Layer 1:

**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

- Application
- Presentation
- Session
- Transport
- Network
- Data Link
- Physical

### Interview Question: Why do we use the OSI model?

**Answer:** The OSI model helps us understand network communication by dividing it into seven layers. It also makes troubleshooting easier because we can identify which layer may be causing a problem.

---

## 5. TCP/IP Model

The TCP/IP model describes the network architecture used for communication over the internet.

It is commonly represented using four layers.

| TCP/IP Layer | Main Function | Example Protocols |
|---|---|---|
| Application | Application-level communication | HTTP, HTTPS, DNS, SSH, SMTP |
| Transport | End-to-end communication | TCP, UDP |
| Internet | Logical addressing and routing | IPv4, IPv6, ICMP |
| Network Access | Local network and physical transmission | Ethernet, Wi-Fi |

### OSI Model vs TCP/IP Model

| OSI Model | TCP/IP Model |
|---|---|
| Has 7 layers | Commonly has 4 layers |
| Reference model | Practical protocol architecture |
| Separates session and presentation layers | Includes these functions in the application layer |
| Separates physical and data link layers | Combines them in network access |

**Interview Answer:** The OSI model has seven layers and is mainly used as a reference for understanding network communication. The TCP/IP model commonly has four layers and describes the architecture used by internet protocols.

---

## 6. IP Address

An IP (Internet Protocol) address is a logical address assigned to a network interface so that devices can communicate over an IP network.

**Example:** `192.168.1.10`

IP addresses help identify the source and destination of network packets.

There are two main versions of IP:

- IPv4
- IPv6

### 6.1 IPv4

IPv4 uses a 32-bit address.

It is normally written as four decimal numbers separated by dots.

Example:

```text

```text
IP address:  192.168.1.10
Subnet mask: 255.255.255.0
```

In this example, the network is `192.168.1.0/24`, and the host address is `192.168.1.10`.

### 9.2 What Is CIDR?

CIDR stands for Classless Inter-Domain Routing.

It represents an IP network using an IP address followed by a slash and a prefix length.

Example:

```text
192.168.1.0/24
```

`/24` means that the first 24 bits represent the network portion.

### Common CIDR Examples

| CIDR | Subnet Mask | Total IPv4 Addresses |
|---|---|---:|
| `/8` | `255.0.0.0` | 16,777,216 |
| `/16` | `255.255.0.0` | 65,536 |
| `/24` | `255.255.255.0` | 256 |
| `/25` | `255.255.255.128` | 128 |
| `/26` | `255.255.255.192` | 64 |
| `/27` | `255.255.255.224` | 32 |
| `/28` | `255.255.255.240` | 16 |
| `/30` | `255.255.255.252` | 4 |

For a conventional IPv4 subnet, two addresses are reserved for the network and broadcast addresses. Therefore, a `/24` subnet normally has 254 usable host addresses. Some special-purpose networks and cloud platforms have different reservation rules.

### 9.3 Basic Subnetting Example

Given:

```text
Network: 192.168.1.0/24
```

- Network address: `192.168.1.0`
- First usable host: `192.168.1.1`
- Last usable host: `192.168.1.254`
- Broadcast address: `192.168.1.255`
- Total addresses: 256
- Usable host addresses: 254

### 9.4 Why Is Subnetting Important?

Subnetting divides a large network into smaller networks.

Benefits:
- Better IP address management.
- Network segmentation.
- Reduced broadcast domains in IPv4 LANs.
- Improved security boundaries.
- Easier infrastructure organization.

**DevOps example:** In AWS, you can divide a VPC into public and private subnets to separate internet-facing resources from internal application and database resources.

---

## 10. MAC Address

A MAC (Media Access Control) address is a link-layer address used to identify a network interface on a local network.

Example:

```text
00:1A:2B:3C:4D:5E
```

A typical Ethernet MAC address is 48 bits long.

### IP Address vs MAC Address

| IP Address | MAC Address |
|---|---|
| Logical address | Link-layer address |
| Used for routing between networks | Used for local network frame delivery |
| IPv4 or IPv6 | Common Ethernet MAC is 48 bits |
| Can change when network configuration changes | Usually assigned to a network interface, but can be changed or randomized |

---

## 11. ARP – Address Resolution Protocol

ARP is used in IPv4 local networks to discover the MAC address associated with a known local IPv4 address.

**Example:**

A computer wants to send data to `192.168.1.20`, but it does not know that device's MAC address.

1. The computer checks its ARP cache.
2. If no entry exists, it sends an ARP request on the local network.
3. The device with that IP address replies with its MAC address.
4. The sender stores the mapping temporarily and sends the Ethernet frame.

Useful command:

```bash
ip neigh
```

This displays the local neighbor table, which includes IPv4 ARP and IPv6 neighbor information.

---

## 12. DNS – Domain Name System

DNS stands for Domain Name System.

DNS translates human-readable domain names into IP addresses so clients can locate servers.

**Example:**

```text
www.google.com
```

A DNS lookup can return IP addresses associated with that domain.

### How DNS Works

1. A user enters a domain name in a browser.
2. The operating system or browser checks its DNS cache.
3. If needed, a DNS resolver is queried.
4. The resolver obtains the required DNS record, using its cache or querying DNS servers.
5. The resolver returns the result to the client.
6. The client connects to the destination IP address.

### Common DNS Record Types

| Record | Purpose |
|---|---|
| A | Maps a name to an IPv4 address |
| AAAA | Maps a name to an IPv6 address |
| CNAME | Creates an alias to another domain name |
| MX | Identifies mail servers |
| TXT | Stores text information, often used for verification and email security |
| NS | Identifies authoritative name servers |
| PTR | Used for reverse DNS lookup |

### Useful DNS Commands

```bash
nslookup google.com
```

```bash
dig google.com
```

```bash
host google.com
```

**DevOps scenario:** If a website cannot be accessed by its domain name but works using its IP address, investigate DNS resolution, DNS records, and resolver configuration.

---

## 13. DHCP – Dynamic Host Configuration Protocol

DHCP automatically provides network configuration to devices.

It can assign:
- IP address.
- Subnet mask or prefix.
- Default gateway.
- DNS server addresses.
- Lease duration.

### DHCP DORA Process

DORA represents four steps:

1. **Discover** – Client searches for a DHCP server.
2. **Offer** – DHCP server offers an IP configuration.
3. **Request** – Client requests the offered configuration.
4. **Acknowledge** – Server confirms the lease.

### Interview Question: What is the difference between DNS and DHCP?

**Answer:** DNS resolves domain names to IP addresses. DHCP automatically assigns IP configuration, such as IP addresses, gateways, and DNS server information, to network devices.

---

## 14. TCP and UDP

TCP and UDP are transport-layer protocols.

### 14.1 TCP – Transmission Control Protocol

TCP is a connection-oriented transport protocol.

Features:
- Establishes a connection before normal data transfer.
- Provides reliable, ordered byte-stream delivery.
- Uses acknowledgments and retransmissions.
- Provides flow control and congestion control.

**Examples:** HTTP/1.1, HTTP/2, SSH, and many database connections commonly use TCP.

### TCP Three-Way Handshake

TCP establishes a connection using three steps:

1. SYN – Client requests a connection.
2. SYN-ACK – Server acknowledges and responds.
3. ACK – Client acknowledges the response.

After this handshake, data transfer can begin.

### 14.2 UDP – User Datagram Protocol

UDP is a connectionless transport protocol.

Features:
- No connection handshake before sending datagrams.
- No built-in guarantee of delivery.
- No built-in guarantee of ordering.
- Lower protocol overhead than TCP.

**Examples:** DNS queries commonly use UDP; real-time voice and video applications may use UDP.

### TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Reliable, ordered byte stream | No built-in delivery or ordering guarantee |
| Uses acknowledgments and retransmissions | No built-in acknowledgments or retransmissions |
| More protocol overhead | Less protocol overhead |
| Common for SSH and web traffic | Common for DNS queries and real-time traffic |

**Interview Answer:** TCP is used when reliable, ordered delivery is important. UDP is used when lower overhead and timely delivery are preferred and the application can tolerate or handle packet loss.

---

## 15. Port Numbers

A port number identifies a specific service or application endpoint on a host.

TCP and UDP use port numbers to direct network traffic to the appropriate application.

Port numbers range from 0 to 65535.

### Important Port Numbers for DevOps

| Port | Protocol / Service | Purpose |
|---:|---|---|
| 20, 21 | FTP | File transfer |
| 22 | SSH | Secure remote login |
| 23 | Telnet | Unencrypted remote login |
| 25 | SMTP | Email transfer |
| 53 | DNS | Domain name resolution |
| 67, 68 | DHCP | Dynamic host configuration |
| 80 | HTTP | Web traffic |
| 110 | POP3 | Email retrieval |
| 123 | NTP | Time synchronization |
| 143 | IMAP | Email retrieval |
| 161 | SNMP | Network management |
| 443 | HTTPS | Encrypted web traffic |
| 3306 | MySQL | Database service |
| 5432 | PostgreSQL | Database service |
| 6379 | Redis | In-memory data store |
| 8080 | Common alternate HTTP port | Application or proxy traffic |
| 27017 | MongoDB | Database service |

**Important:** A service can be configured to listen on a non-default port. Always verify the actual configuration.

### Interview Question: What is the difference between an IP address and a port number?

**Answer:** An IP address identifies a network interface or destination host, while a port number identifies the application or service endpoint receiving the traffic on that host.

---

## 16. HTTP and HTTPS

### 16.1 HTTP

HTTP stands for Hypertext Transfer Protocol.

It is an application-layer protocol used for communication between clients and web servers.

Default port: `80`

### 16.2 HTTPS

HTTPS stands for Hypertext Transfer Protocol Secure.

It is HTTP communication protected by TLS (Transport Layer Security).

Default port: `443`

TLS helps provide:
- Encryption.
- Integrity protection.
- Server authentication through certificates.

### HTTP vs HTTPS

| HTTP | HTTPS |
|---|---|
| No TLS protection by itself | Uses TLS |
| Default port 80 | Default port 443 |
| Traffic is not encrypted by HTTP itself | Traffic is encrypted in transit |
| Does not authenticate the server using TLS certificates | TLS certificates help authenticate the server |

### Common HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve a resource |
| POST | Submit data or request processing |
| PUT | Create or replace a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |
| HEAD | Retrieve response headers without the response body |

### Common HTTP Status Codes

| Code | Meaning |
|---:|---|
| 200 | OK |
| 201 | Created |
| 301 | Moved Permanently |
| 302 | Found / Temporary Redirect |
| 400 | Bad Request |
| 401 | Unauthorized – authentication is required or failed |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |
| 504 | Gateway Timeout |

**DevOps example:** If Nginx returns `502 Bad Gateway`, check whether the upstream application is running, listening on the expected port, and reachable from Nginx.

---

## 17. Routing and Default Gateway

### 17.1 Routing

Routing is the process of selecting a path for packets to travel from a source network to a destination network.

Routers use routing tables to decide where to forward packets.

### 17.2 Default Gateway

A default gateway is the next-hop device used when a destination is not covered by a more specific route in the routing table.

**Example:** A laptop sends traffic to a device on another network through its home router.

Useful command:

```bash
ip route
```

This displays the routing table on a Linux system.

### 17.3 Switch vs Router

| Switch | Router |
|---|---|
| Connects devices within a LAN | Connects different IP networks |
| Commonly forwards using MAC addresses | Forwards using IP routing |
| Commonly operates at Layer 2 | Commonly operates at Layer 3 |

---

## 18. Firewall and Network Security

A firewall controls network traffic according to security rules.

It can filter traffic by:
- Source IP.
- Destination IP.
- Protocol.
- Port.
- Connection state.

### Inbound and Outbound Rules

**Inbound traffic:** Traffic entering a server or network.

**Outbound traffic:** Traffic leaving a server or network.

**Example:** A web server might allow inbound TCP traffic on ports 80 and 443 while restricting SSH access to trusted administrator IP addresses.

### DevOps Security Best Practices

- Expose only required ports.
- Avoid opening SSH to every IP address when possible.
- Use HTTPS for web applications.
- Keep private databases inaccessible from the public internet.
- Review firewall and cloud security group rules.
- Use least-privilege access.
- Monitor unusual network traffic.

---

## 19. Important Linux Networking Commands

These commands are especially useful for Linux and DevOps troubleshooting.

### 19.1 Display IP Addresses

```bash
ip addr
```

Displays IP addresses and network interfaces.

Short form:

```bash
ip a
```

### 19.2 Display Network Interfaces

```bash
ip link
```

Displays network interfaces and their link state.

### 19.3 Display Routing Table

```bash
ip route
```

Shows routes and the default gateway.

### 19.4 Test Connectivity with Ping

```bash
ping google.com
```

Sends ICMP echo requests to test reachability and measure round-trip time.

Stop it using:

```text
Ctrl + C
```

**Note:** A failed ping does not always mean a server is unavailable. Firewalls may block ICMP while allowing application traffic.

### 19.5 Check DNS Resolution

```bash
nslookup google.com
```

Or:

```bash
dig google.com
```

### 19.6 Check Listening Ports

```bash
ss -tuln
```

Options:
- `-t` – TCP sockets.
- `-u` – UDP sockets.
- `-l` – Listening sockets.
- `-n` – Numeric addresses and port numbers.

To display process information as well:

```bash
sudo ss -tulnp
```

### 19.7 Check Active Connections

```bash
ss -tun
```

Displays TCP and UDP sockets, including non-listening sockets.

### 19.8 Test a Port with Netcat

```bash
nc -zv 192.168.1.10 22
```

This checks whether a TCP connection to port 22 can be established.

### 19.9 Test an HTTP Endpoint

```bash
curl -I https://example.com
```

Displays response headers.

To view more connection details:

```bash
curl -v https://example.com
```

### 19.10 Trace Network Path

```bash
traceroute google.com
```

Shows the network hops toward a destination.

On some Linux distributions, the command may need to be installed first.

Alternative:

```bash
tracepath google.com
```

### 19.11 Display Neighbor Information

```bash
ip neigh
```

Displays IPv4 and IPv6 neighbor entries.

### 19.12 Display Hostname

```bash
hostname
```

### 19.13 Display DNS Resolver Configuration

```bash
cat /etc/resolv.conf
```

This shows resolver configuration. On systems using `systemd-resolved` or another network manager, the file may be generated or point to a local resolver.

### 19.14 Inspect Network Interfaces with NetworkManager

On systems using NetworkManager:

```bash
nmcli device status
```

To show connection details:

```bash
nmcli connection show
```

### Quick Command Reference

| Command | Purpose |
|---|---|
| `ip addr` | Display IP addresses |
| `ip link` | Display network interfaces |
| `ip route` | Display routing table |
| `ping` | Test ICMP reachability |
| `ss -tuln` | Display listening TCP/UDP ports |
| `nslookup` | Perform DNS lookup |
| `dig` | Query DNS records |
| `curl` | Test HTTP/HTTPS requests |
| `nc` | Test network connections |
| `traceroute` | Trace network path |
| `ip neigh` | Display neighbor table |
| `hostname` | Display system hostname |

---

## 20. Networking in AWS and DevOps

Networking is a major part of cloud infrastructure.

### 20.1 VPC – Virtual Private Cloud

An AWS VPC is a logically isolated virtual network where you launch AWS resources.

You can configure:
- IP address ranges.
- Subnets.
- Route tables.
- Internet gateways.
- NAT gateways.
- Security groups.
- Network ACLs.

### 20.2 Public Subnet

A public subnet has a route to an internet gateway for internet connectivity.

A resource also needs suitable addressing and security rules to communicate with the internet.

**Example:** A public-facing load balancer or web server may be deployed in a public subnet.

### 20.3 Private Subnet

A private subnet does not have a direct route to an internet gateway for direct internet access.

**Example:** An application server or database may be placed in a private subnet.

### 20.4 Internet Gateway

An internet gateway allows communication between a VPC and the internet when routing, addressing, and security rules are correctly configured.

### 20.5 NAT Gateway

A NAT gateway allows resources in a private subnet to initiate supported connections to external networks, such as the internet, without allowing unsolicited internet connections to those private resources through the NAT gateway.

### 20.6 Security Group

An AWS security group acts as a stateful virtual firewall for associated resources.

It controls inbound and outbound traffic.

### 20.7 Network ACL

A network ACL is a stateless firewall applied at the subnet level.

It evaluates inbound and outbound traffic rules separately.

### Security Group vs Network ACL

| Security Group | Network ACL |
|---|---|
| Applies to associated network interfaces/resources | Applies at subnet level |
| Stateful | Stateless |
| Allows rules only | Supports allow and deny rules |
| Return traffic is automatically allowed for permitted connections | Return traffic must be allowed by the relevant rules |

### DevOps Example: Three-Tier Application

A common architecture separates resources into tiers:

1. **Public tier:** Load balancer receives user requests.
2. **Application tier:** Application servers process requests in private subnets.
3. **Database tier:** Database servers remain in private subnets and accept traffic only from the application tier.

This design reduces unnecessary public exposure.

---

## 21. Practical DevOps Networking Troubleshooting

Networking troubleshooting should be systematic. Do not randomly change firewall or server settings.

### Scenario 1: Website Is Not Opening

Possible causes:
- DNS resolution failure.
- Server is down.
- Web service is stopped.
- Port 80 or 443 is blocked.
- Routing issue.
- TLS or certificate issue.

Useful checks:

```bash
ping example.com
```

```bash
nslookup example.com
```

```bash
curl -Iv https://example.com
```

```bash
ss -tuln
```

### Scenario 2: Application Is Not Accessible on Port 8080

Check whether the application is listening:

```bash
sudo ss -tulnp | grep 8080
```

If nothing is listening, check the application process and service configuration.

Test locally:

```bash
curl http://localhost:8080
```

Test from another machine:

```bash
nc -zv SERVER_IP 8080
```

Also inspect host firewall and cloud security rules.

### Scenario 3: Domain Name Is Not Resolving

Check DNS:

```bash
nslookup example.com
```

Or:

```bash
dig example.com
```

If resolution fails, investigate:
- DNS record configuration.
- Resolver settings.
- DNS server availability.
- DNS cache and propagation where relevant.

### Scenario 4: Server Is Reachable but Application Is Not Working

Check:

```bash
ping SERVER_IP
```

```bash
sudo ss -tulnp
```

```bash
curl -v http://localhost:8080
```

Check application logs and firewall rules.

A successful ping only tests ICMP reachability. It does not prove that the application port is working.

### Scenario 5: Nginx Returns 502 Bad Gateway

Check the application service:

```bash
sudo systemctl status app.service
```

Check listening ports:

```bash
sudo ss -tulnp
```

Test the upstream locally:

```bash
curl http://127.0.0.1:8080
```

Inspect Nginx logs:

```bash
sudo tail -f /var/log/nginx/error.log
```

Possible causes:
- Application service is stopped.
- Nginx uses the wrong upstream port.
- Application is listening on a different interface.
- Nginx cannot connect to the upstream.
- Application is overloaded or failing.

### Scenario 6: SSH Connection Is Failing

Check:
- Server IP address.
- SSH service status.
- Port 22 listening state.
- Firewall and security group rules.
- Network route.
- SSH key and username.

Commands:

```bash
sudo systemctl status ssh
```

On some distributions the service is named `sshd`:

```bash
sudo systemctl status sshd
```

Test port connectivity:

```bash
nc -zv SERVER_IP 22
```

### Recommended Troubleshooting Order

1. Confirm the destination IP or domain name.
2. Check DNS resolution if using a domain.
3. Check basic network reachability where ICMP is permitted.
4. Check the route and gateway.
5. Check whether the required service is running.
6. Check whether the service is listening on the expected port and interface.
7. Test the port from the client.
8. Inspect firewall and cloud security rules.
9. Review application and system logs.

---

## 22. Hands-On Networking Exercises

Practice these commands on your Linux machine.

### Exercise 1: Find Your IP Address

```bash
ip addr
```

Identify:
- Network interface name.
- IPv4 address.
- IPv6 address, if configured.
- Interface state.

### Exercise 2: Find Your Default Gateway

```bash
ip route
```

Identify the line beginning with `default`.

### Exercise 3: Test Connectivity

```bash
ping -c 4 google.com
```

Observe whether packets are received and check the round-trip time.

### Exercise 4: Perform DNS Lookup

```bash
nslookup google.com
```

Find the DNS server and returned IP addresses.

### Exercise 5: Check Listening Ports

```bash
ss -tuln
```

Identify the listening TCP and UDP ports.

### Exercise 6: Test an HTTP Website

```bash
curl -I https://example.com
```

Identify the HTTP status code.

### Exercise 7: Check a Port

```bash
nc -zv google.com 443
```

Observe whether the TCP connection succeeds. Network policies may affect the result.

### Exercise 8: Trace the Route

```bash
traceroute google.com
```

Observe the hops. Some routers may not respond to traceroute probes.

---

## 23. Networking Interview Questions and Answers

### Q1. What is computer networking?

Computer networking is the process of connecting two or more devices so they can communicate and share resources.

### Q2. What is an IP address?

An IP address is a logical address assigned to a network interface to identify it and enable communication over an IP network.

### Q3. What is the difference between IPv4 and IPv6?

IPv4 uses 32-bit addresses, while IPv6 uses 128-bit addresses. IPv6 provides a much larger address space.

### Q4. What is a private IP address?

A private IP address is used within private networks and is not directly routable over the public internet.

### Q5. What is a public IP address?

A public IP address is globally routable and can be used to identify a network endpoint on the internet.

### Q6. What is DNS?

DNS translates domain names into IP addresses so clients can locate servers.

### Q7. What is DHCP?

DHCP automatically assigns network configuration, such as IP addresses, subnet masks, gateways, and DNS servers, to devices.

### Q8. What is the OSI model?

The OSI model is a seven-layer reference model that explains how data communication takes place across a network.

### Q9. Name the seven OSI layers.

Application, Presentation, Session, Transport, Network, Data Link, and Physical.

### Q10. What is the TCP/IP model?

The TCP/IP model is a network architecture commonly represented by four layers: Application, Transport, Internet, and Network Access.

### Q11. What is TCP?

TCP is a connection-oriented transport protocol that provides reliable and ordered byte-stream delivery.

### Q12. What is UDP?

UDP is a connectionless transport protocol that sends datagrams without built-in delivery or ordering guarantees.

### Q13. What is the difference between TCP and UDP?

TCP provides reliable, ordered delivery using acknowledgments and retransmissions. UDP has lower overhead and does not provide those guarantees by itself.

### Q14. What is a port number?

A port number identifies an application or service endpoint on a host.

### Q15. What is the default port for SSH?

Port 22.

### Q16. What is the default port for HTTP and HTTPS?

HTTP uses port 80 by default, and HTTPS uses port 443 by default.

### Q17. What is a subnet mask?

A subnet mask identifies the network and host portions of an IPv4 address.

### Q18. What is CIDR?

CIDR is a method of representing IP networks using a prefix length, such as `192.168.1.0/24`.

### Q19. What is a MAC address?

A MAC address is a link-layer address used for communication within a local network.

### Q20. What is ARP?

ARP discovers the MAC address associated with a local IPv4 address.

### Q21. What is a router?

A router connects different IP networks and forwards packets using routing information.

### Q22. What is a switch?

A switch connects devices within a local network and commonly forwards Ethernet frames using MAC addresses.

### Q23. What is a default gateway?

A default gateway is the next-hop device used when a destination does not match a more specific route.

### Q24. What is a firewall?

A firewall controls network traffic according to security rules.

### Q25. What is NAT?

NAT translates network addresses, commonly allowing devices using private IP addresses to communicate with external networks through a public address.

### Q26. What is the TCP three-way handshake?

It is the TCP connection establishment process involving SYN, SYN-ACK, and ACK.

### Q27. What is the difference between HTTP and HTTPS?

HTTP does not provide TLS protection by itself. HTTPS uses TLS to protect HTTP communication through encryption and integrity protection and to authenticate the server using certificates.

### Q28. Which Linux command displays IP addresses?

```bash
ip addr
```

### Q29. Which Linux command displays the routing table?

```bash
ip route
```

### Q30. Which Linux command checks listening ports?

```bash
ss -tuln
```

### Q31. How do you troubleshoot DNS issues?

Use `nslookup` or `dig` to check whether the domain resolves. Then inspect DNS records, resolver configuration, and DNS server availability.

### Q32. How do you check whether port 8080 is listening?

```bash
sudo ss -tulnp | grep 8080
```

### Q33. A website is not opening. What would you check?

I would check DNS resolution, network reachability, routing, whether the web service is running, listening ports, firewall rules, TLS configuration, and application logs.

### Q34. What is a public subnet in AWS?

A public subnet has a route to an internet gateway. Resources also need appropriate IP addressing and security rules for internet communication.

### Q35. What is a private subnet in AWS?

A private subnet does not have a direct route to an internet gateway for direct internet access. It is commonly used for internal application servers and databases.

### Q36. What is the difference between a security group and a network ACL?

A security group is stateful and applies to associated resources or network interfaces. A network ACL is stateless and applies at the subnet level.

### Q37. What does HTTP 502 mean?

502 Bad Gateway means a gateway or proxy received an invalid response from an upstream server or could not obtain a valid upstream response.

### Q38. What does HTTP 504 mean?

504 Gateway Timeout means a gateway or proxy did not receive a timely response from an upstream server.

### Q39. What is a load balancer?

A load balancer distributes incoming traffic across multiple backend servers to improve availability and scalability.

### Q40. Why is networking important for DevOps?

Networking enables communication between applications, servers, containers, databases, and cloud services. DevOps engineers need it to configure infrastructure, secure services, and troubleshoot connectivity issues.

---

## 24. Important Points to Remember

- Networking connects devices so they can communicate.
- LAN covers a limited area; WAN covers larger geographical areas.
- The OSI model has seven layers.
- The TCP/IP model is commonly represented with four layers.
- IPv4 uses 32 bits; IPv6 uses 128 bits.
- Public IP addresses are globally routable; private IP addresses are used within private networks.
- DNS resolves domain names to IP addresses.
- DHCP automatically assigns network configuration.
- TCP provides reliable, ordered delivery; UDP does not provide built-in delivery guarantees.
- IP addresses identify network endpoints; port numbers identify application endpoints.
- HTTP commonly uses port 80; HTTPS commonly uses port 443; SSH commonly uses port 22.
- `ip addr` displays IP configuration.
- `ip route` displays routing information.
- `ss -tuln` displays listening TCP and UDP sockets.
- `dig` and `nslookup` help troubleshoot DNS.
- AWS VPCs, subnets, route tables, security groups, and network ACLs are important DevOps networking concepts.

**Learning goal:** Understand the concepts, practise the Linux commands, and explain how you would troubleshoot a real application connectivity problem in a DevOps environment.
