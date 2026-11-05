# Linux Networking Commands for DevOps

## 1. Introduction

Linux networking commands help administrators and DevOps engineers inspect network configuration, test connectivity, troubleshoot DNS, check open ports, and analyze network traffic.

Networking commands are useful for:
- Checking IP addresses and network interfaces.
- Testing connectivity between servers.
- Troubleshooting DNS resolution.
- Checking listening ports and active connections.
- Testing HTTP and HTTPS services.
- Finding network routing problems.
- Monitoring network traffic.
- Troubleshooting application and cloud connectivity.

---

## 2. Display Network Interfaces – `ip`

The `ip` command is used to display and manage network interfaces, IP addresses, routes, and neighbor information.

### 2.1 Display IP Addresses

```bash
ip addr
```

Short form:

```bash
ip a
```

This command displays:
- Network interface names.
- IPv4 addresses.
- IPv6 addresses.
- Interface state.
- MAC addresses.

Example output:

```text
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    link/ether 00:11:22:33:44:55
    inet 192.168.1.10/24
```

Explanation:
- `eth0` – Network interface name.
- `UP` – Interface is administratively enabled.
- `LOWER_UP` – Physical or virtual link is detected as operational.
- `link/ether` – MAC address.
- `inet` – IPv4 address.
- `192.168.1.10/24` – IPv4 address with prefix length.

### 2.2 Display Network Interfaces

```bash
ip link
```

Displays network interfaces and their link status.

### 2.3 Display a Specific Interface

```bash
ip addr show eth0
```

Replace `eth0` with your actual interface name.

### 2.4 Enable a Network Interface

```bash
sudo ip link set eth0 up
```

### 2.5 Disable a Network Interface

```bash
sudo ip link set eth0 down
```

**Important:** Disabling an interface can disconnect your SSH session. Be careful when managing a remote server.

### 2.6 Display Routing Table

```bash
ip route
```

Displays the system's routing table.

Example:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link
```

Explanation:
- `default` – Default route.
- `via 192.168.1.1` – Default gateway.
- `dev eth0` – Interface used for the route.

### 2.7 Display Neighbor Table

```bash
ip neigh
```

Displays neighbor information, including IPv4 ARP and IPv6 neighbor-discovery entries.

### DevOps Use Case

If a Linux server cannot reach another network, inspect its IP configuration and routing table:

```bash
ip addr
ip route
```

Check whether the interface is enabled and whether an appropriate route exists.

---

## 3. Test Network Connectivity – `ping`

The `ping` command tests IP reachability using ICMP echo requests and replies.

Syntax:

```bash
ping destination
```

Example:

```bash
ping google.com
```

### 3.1 Send a Fixed Number of Packets

```bash
ping -c 4 google.com
```

`-c 4` sends four echo requests and then stops.

### 3.2 Set a Time Interval

```bash
ping -i 2 google.com
```

Sends requests at an interval of two seconds.

### 3.3 Set Packet Size

```bash
ping -s 100 google.com
```

Sends an ICMP payload of 100 bytes.

### 3.4 Ping a Server Using Its IP

```bash
ping 8.8.8.8
```

### Understanding Ping Output

Example:

```text
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=20 ms
```

- `64 bytes` – Size of the received packet.
- `icmp_seq=1` – Sequence number.
- `ttl=117` – Time to live remaining.
- `time=20 ms` – Round-trip time.

### Important Note

A failed ping does not always mean the server is down. A firewall or network policy may block ICMP while allowing application traffic.

### DevOps Use Case

If an application server cannot reach another server:

```bash
ping -c 4 SERVER_IP
```

Check whether ICMP replies are received. If not, investigate routes, firewall rules, and whether ICMP is permitted.

---

## 4. DNS Troubleshooting Commands

DNS commands help check whether a domain name resolves to an IP address.

### 4.1 `nslookup`

`nslookup` queries DNS servers to retrieve DNS information.

Syntax:

```bash
nslookup domain
```

Example:

```bash
nslookup google.com
```

It displays:
- DNS server used.
- Domain name queried.
- Returned IP address or addresses.

### Query a Specific DNS Server

```bash
nslookup google.com 8.8.8.8
```

This asks the DNS server at `8.8.8.8` to resolve the domain.

### 4.2 `dig`

`dig` stands for Domain Information Groper.

It provides detailed DNS query information.

Example:

```bash
dig google.com
```

### Display Only the Answer

```bash
dig +short google.com
```

### Query an A Record

```bash
dig google.com A
```

### Query an AAAA Record

```bash
dig google.com AAAA
```

### Query a Specific DNS Server

```bash
dig @8.8.8.8 google.com
```

### 4.3 `host`

The `host` command performs DNS lookups.

Example:

```bash
host google.com
```

### Compare DNS Commands

| Command | Purpose |
|---|---|
| `nslookup` | Performs DNS lookups |
| `dig` | Provides detailed DNS query information |
| `host` | Performs simple DNS lookups |

### DevOps Scenario

Your website is not opening using its domain name.

Check:

```bash
nslookup example.com
```

Then:

```bash
dig +short example.com
```

If the domain does not resolve, investigate DNS records, resolver configuration, and DNS server availability.

---

## 5. Check Listening Ports and Connections – `ss`

The `ss` command displays socket information.

It is commonly used to inspect listening ports and active network connections.

### 5.1 Display Listening TCP and UDP Ports

```bash
ss -tuln
```

Options:
- `-t` – TCP sockets.
- `-u` – UDP sockets.
- `-l` – Listening sockets.
- `-n` – Numeric addresses and port numbers.

### 5.2 Display Process Information

```bash
sudo ss -tulnp
```

The `-p` option displays process information when available.

### 5.3 Display TCP Sockets

```bash
ss -t
```

### 5.4 Display UDP Sockets

```bash
ss -u
```

### 5.5 Display Established TCP Connections

```bash
ss -tn state established
```

### 5.6 Check a Specific Port

```bash
sudo ss -tulnp | grep :8080
```

This helps determine whether a process is listening on port 8080.

### Example Output

```text
LISTEN 0 128 0.0.0.0:22 0.0.0.0:*
```

This indicates a service is listening on TCP port 22 on all IPv4 interfaces.

### DevOps Scenario

Your application is expected to run on port 8080, but users cannot access it.

Run:

```bash
sudo ss -tulnp | grep :8080
```

If no matching listener exists, check whether the application started successfully and whether it is configured to use the expected port.

---

## 6. Older Socket Command – `netstat`

`netstat` displays network connections, routing tables, and network interface statistics.

It belongs to the older `net-tools` package and may not be installed by default on modern Linux distributions.

### 6.1 Display Listening Ports

```bash
netstat -tuln
```

### 6.2 Display Listening Ports with Process Information

```bash
sudo netstat -tulnp
```

### 6.3 Display Routing Table

```bash
netstat -r
```

### 6.4 Display Network Statistics

```bash
netstat -s
```

### `ss` vs `netstat`

| `ss` | `netstat` |
|---|---|
| Modern socket utility | Older networking utility |
| Commonly available on modern Linux systems | May require `net-tools` |
| Often preferred for socket inspection | Still encountered in older environments |

**Interview Tip:** Know both commands, but prefer `ss` on modern Linux systems.

---

## 7. Test Network Connections – `nc`

`nc` stands for Netcat.

It is used to read and write data over network connections. It is also useful for checking whether a TCP port is reachable.

### 7.1 Check Whether a TCP Port Is Reachable

```bash
nc -zv 192.168.1.10 22
```

Options:
- `-z` – Scan for listening services without sending application data.
- `-v` – Verbose output.

### 7.2 Check an Application Port

```bash
nc -zv SERVER_IP 8080
```

### 7.3 Check an HTTPS Port

```bash
nc -zv example.com 443
```

### 7.4 Check a Port with a Timeout

```bash
nc -zv -w 3 SERVER_IP 8080
```

`-w 3` sets a three-second timeout for the connection attempt.

### Understanding Results

A successful connection generally means the TCP connection could be established.

A failed connection may indicate:
- Service is not listening.
- Firewall blocks the connection.
- Network route is unavailable.
- Destination host is unreachable.

**Important:** A successful TCP connection does not prove that the application is healthy or returning correct responses.

### DevOps Scenario

You need to check whether an application server is reachable on port 8080:

```bash
nc -zv 10.0.1.20 8080
```

If the connection fails, check the service, routing, and firewall or security group rules.

---

## 8. HTTP and HTTPS Testing – `curl`

`curl` transfers data using URLs and supports protocols such as HTTP and HTTPS.

It is very useful for testing APIs and web servers.

### 8.1 Send a GET Request

```bash
curl https://example.com
```

Displays the response body.

### 8.2 Display Only Response Headers

```bash
curl -I https://example.com
```

`-I` sends a HEAD request and displays response headers.

### 8.3 Display Detailed Connection Information

```bash
curl -v https://example.com
```

Useful for debugging:
- DNS resolution.
- TCP connection.
- TLS handshake.
- HTTP request and response.

### 8.4 Display Only the HTTP Status Code

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://example.com
```

Useful when checking whether a website returns a successful HTTP response.

### 8.5 Test a Local Application

```bash
curl http://localhost:8080
```

-d '{"name":"Prathyusha"}'

```

This sends a POST request with a JSON body.


### 8.7 Follow Redirects


```bash
curl -L http://example.com

```


`-L` follows HTTP redirects.


### DevOps Scenario


You deployed an application and want to verify whether it is responding:


```bash
curl -I http://localhost:8080

```


If the application returns an unexpected status code, inspect application logs and server configuration.


---


## 9. Download Files – `wget`

`wget` is a command-line utility used to download files from web servers.


### 9.1 Download a File

```bash
wget https://example.com/file.zip
```

### 9.2 Save with a Different Filename

```bash
wget -O backup.zip https://example.com/file.zip
```

### 9.3 Continue an Interrupted Download


```bash

wget -c https://example.com/file.zip

```


### 9.4 Download in the Background

```bash

wget -b https://example.com/file.zip

```


### `curl` vs `wget`


| curl | wget |
|---|---|

| Supports many protocols and API interactions | Commonly used for downloading files |
| Useful for testing HTTP APIs | Useful for downloading files |

| Can display or send HTTP requests | Supports download continuation and recursive retrieval |

| Often used in DevOps health checks | Often used in scripts to fetch files |

---

## 10. Trace Network Routes – `traceroute`

`traceroute` identifies the intermediate network hops between your machine and a destination.

Example:

```bash
traceroute google.com
```

It helps investigate where packets travel and where delays or failures may occur.

### 10.1 Trace a Specific IP

```bash
traceroute 8.8.8.8
```

### 10.2 Use TCP Probes

On implementations that support it:

```bash
traceroute -T -p 443 example.com
```

This uses TCP probes toward port 443.

### 10.3 Alternative – `tracepath`

```bash
tracepath google.com
```

`tracepath` can trace the route and provide path MTU information on supported systems.

### Important Note

Some routers do not respond to traceroute probes. Asterisks in the output do not necessarily mean that the destination is unreachable.

### DevOps Scenario

A server can connect to its local gateway but cannot reach an external service.

Use:

```bash
traceroute example.com
```

Inspect the path for possible routing problems. Confirm with other tests because intermediate routers may filter probes.

---

## 11. Network Traffic Analysis – `tcpdump`

`tcpdump` captures and displays packets on a network interface.

It is useful for troubleshooting network communication and inspecting traffic.

### 11.1 Capture Packets on an Interface

```bash
sudo tcpdump -i eth0
```

Replace `eth0` with the actual interface name.

### 11.2 Capture a Limited Number of Packets

```bash
sudo tcpdump -i eth0 -c 10
```

Captures ten packets and then exits.

### 11.3 Capture Traffic for a Specific Host

```bash
sudo tcpdump -i eth0 host 192.168.1.10
```

### 11.4 Capture Traffic for a Specific Port

```bash
sudo tcpdump -i eth0 port 80
```

### 11.5 Capture TCP Traffic

```bash
sudo tcpdump -i eth0 tcp
```

### 11.6 Capture DNS Traffic

```bash
sudo tcpdump -i eth0 port 53
```

### 11.7 Save Captured Packets to a File

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

### 11.8 Read a Saved Capture

```bash
tcpdump -r capture.pcap
```

### Important Options

| Option | Purpose |
|---|---|
| `-i` | Select network interface |
| `-c` | Stop after a specified packet count |
| `-n` | Do not resolve addresses to names |
| `-nn` | Do not resolve addresses or port numbers |
| `-w` | Write packets to a capture file |
| `-r` | Read a capture file |
| `-v` | Verbose output |

**Security note:** Packet captures may contain sensitive information. Capture only what you are authorized to inspect, protect capture files, and avoid exposing secrets.

### DevOps Scenario

An application server is not receiving expected HTTP traffic.

Capture traffic on the relevant interface:

```bash
sudo tcpdump -i eth0 -nn port 8080
```

Check whether packets arrive at the server. Then investigate routing, firewalls, and the application listener.

---

## 12. Network Configuration and Resolver Files

Linux stores or exposes network configuration through several files and system tools. The exact configuration method depends on the distribution and network manager.

### 12.1 View DNS Resolver Configuration

```bash
cat /etc/resolv.conf
```

This displays configured DNS resolvers and search domains.

### 12.2 View Local Hostname Mappings

```bash
cat /etc/hosts
```

The hosts file provides local hostname-to-address mappings.

### 12.3 View Hostname

```bash
hostname
```

### 12.4 View Detailed Hostname Information

```bash
hostnamectl
```

On systems using systemd, this displays hostname and related system information.

### 12.5 NetworkManager Device Status

On systems using NetworkManager:

```bash
nmcli device status
```

### 12.6 NetworkManager Connections

```bash
nmcli connection show
```

### Important Note

Do not assume that editing `/etc/resolv.conf` or `/etc/hosts` is the correct permanent fix for every Linux distribution. NetworkManager, systemd-resolved, DHCP clients, or cloud-init may manage these settings.

---

## 13. Network Statistics – `ip -s` and `/proc/net`

### 13.1 Display Interface Statistics

```bash
ip -s link
```

Displays packet and byte counters, including errors and drops, for network interfaces.

### 13.2 Display Network Device Statistics

```bash
cat /proc/net/dev
```

Shows network interface statistics.

### Why Are These Useful?

They help investigate:
- Packet drops.
- Transmission errors.
- Receive errors.
- Unexpected traffic volume.
- Interface-level problems.

**DevOps scenario:** If an application experiences network instability, inspect interface statistics and correlate them with system and application logs.

---

## 14. Important Networking Commands – Quick Reference

| Command | Purpose |
|---|---|
| `ip addr` | Display IP addresses |
| `ip link` | Display network interfaces |
| `ip route` | Display routing table |
| `ip neigh` | Display neighbor table |
| `ping -c 4 HOST` | Test ICMP reachability |
| `nslookup DOMAIN` | Perform DNS lookup |
| `dig DOMAIN` | Query DNS records |
| `host DOMAIN` | Perform DNS lookup |
| `ss -tuln` | Display listening TCP/UDP sockets |
| `ss -tn state established` | Display established TCP sockets |
| `netstat -tuln` | Display listening sockets using older utility |
| `nc -zv HOST PORT` | Test TCP port connectivity |
| `curl -I URL` | Display HTTP response headers |
| `curl -v URL` | Display detailed connection information |
| `wget URL` | Download a file |
| `traceroute HOST` | Trace network path |
| `tracepath HOST` | Trace path and inspect path MTU |
| `tcpdump -i INTERFACE` | Capture network packets |
| `ip -s link` | Display interface statistics |
| `nmcli device status` | Display NetworkManager devices |

---

## 15. Practical DevOps Troubleshooting Scenarios

### Scenario 1: Server Cannot Reach the Internet

**Problem:** A Linux server cannot access external websites.

Check the interface:

```bash
ip addr
```

Check the route:

```bash
ip route
```

Test gateway reachability:

```bash
ping -c 4 GATEWAY_IP
```

Test external IP reachability:

```bash
ping -c 4 8.8.8.8
```

Check DNS:

```bash
nslookup google.com
```

**Possible causes:**
- Interface is down.
- Default route is missing.
- Gateway is unreachable.

- Outbound firewall rules block traffic.

- DNS resolver is unavailable.


### Scenario 2: Application Is Not Accessible on Port 8080


Check whether the application is listening:

```bash

sudo ss -tulnp | grep :8080

```


Test locally:


```bash

curl -v http://localhost:8080
```

Test the port from a client:


```bash

nc -zv SERVER_IP 8080
```

Inspect firewall and cloud security group rules.

**Possible causes:**
- Application is stopped.
- Application listens on another port.
- Application listens only on loopback.
- Firewall blocks inbound traffic.
- Cloud security rules do not allow the connection.

### Scenario 3: DNS Is Not Working

Run:

```bash
nslookup example.com
```

Then:

```bash
dig example.com
```

Inspect resolver configuration:

```bash
cat /etc/resolv.conf

```


**Possible causes:**

- Incorrect DNS server.

- DNS server is unavailable.
- Incorrect DNS records.

- Resolver configuration problem.
- Network access to DNS is blocked.


### Scenario 4: Nginx Returns 502 Bad Gateway


Check Nginx:


```bash
sudo systemctl status nginx

```


Check application listener:


```bash
sudo ss -tulnp

```

Test the upstream application:

```bash

curl -v http://127.0.0.1:8080
```


Inspect Nginx error logs:


```bash
sudo tail -f /var/log/nginx/error.log
```


**Possible causes:**

- Backend application is stopped.
- Nginx uses the wrong upstream port.
- Application is listening on the wrong interface.
- Nginx cannot connect to the backend.
- Backend application is overloaded or failing.

### Scenario 5: SSH Connection Is Failing

Check whether the SSH service is running:

```bash
sudo systemctl status ssh
```

Some distributions use:

```bash
sudo systemctl status sshd
```

Check the listener:

```bash
sudo ss -tulnp | grep :22
```

Test connectivity:

```bash
nc -zv SERVER_IP 22
```

**Possible causes:**
- SSH service is stopped.
- Port 22 is not listening.
- Firewall blocks SSH.
- Cloud security group blocks SSH.
- Incorrect IP address, username, or SSH key.
- Network route is unavailable.

### Scenario 6: High Network Traffic or Packet Drops

Check interface statistics:

```bash
ip -s link
```

Inspect active connections:

```bash
ss -tun
```

Capture relevant traffic:

```bash
sudo tcpdump -i eth0 -nn
```

Investigate packet drops, errors, unusual connections, and application traffic patterns.

---

## 16. Hands-On Practice Exercises

Perform these exercises on your Linux machine.

### Exercise 1: Find Your IP Address

```bash
ip addr
```

Identify your active network interface and IPv4 address.

### Exercise 2: Find Your Default Gateway

```bash
ip route
```

Identify the default gateway.

### Exercise 3: Test Connectivity

```bash
ping -c 4 google.com
```

Check whether replies are received.

### Exercise 4: Perform DNS Lookup

```bash
nslookup google.com
```

Identify the DNS server and returned address.

### Exercise 5: Query DNS with `dig`

```bash
dig +short google.com
```

Observe the returned addresses.

### Exercise 6: Check Listening Ports

```bash
ss -tuln
```

Identify the listening TCP and UDP ports.

### Exercise 7: Check Port 443

```bash
nc -zv google.com 443
```

Check whether a TCP connection can be established.

### Exercise 8: Test an HTTPS Website

```bash
curl -I https://example.com
```

Identify the HTTP status code.

### Exercise 9: Find Network Interfaces

```bash
ip link
```

Identify the interface names and their states.

### Exercise 10: Inspect Network Statistics

```bash
ip -s link
```

Look for packet counts, errors, and drops.

### Exercise 11: Trace a Network Route

```bash
traceroute google.com
```

Observe the network hops.

### Exercise 12: Capture DNS Packets

```bash
sudo tcpdump -i any -nn port 53
```

In another terminal, perform a DNS lookup:

```bash
nslookup example.com
```

Observe the DNS traffic, if the lookup uses a visible interface and the capture has sufficient permissions.

---

## 17. Networking Interview Questions and Answers

### Q1. Which command displays IP addresses in Linux?

```bash
ip addr
```

It displays network interfaces and their assigned IP addresses.

### Q2. What is the difference between `ip addr` and `ip route`?

`ip addr` displays interface and IP address information. `ip route` displays the system routing table.

### Q3. What is the purpose of the `ping` command?

It tests IP reachability using ICMP echo requests and replies.

### Q4. Which command checks DNS resolution?

Common commands are:

```bash
nslookup google.com
dig google.com
host google.com
```

### Q5. What is the difference between `nslookup` and `dig`?

Both query DNS. `dig` provides more detailed query and response information, while `nslookup` is commonly used for straightforward lookups.

### Q6. Which command displays listening ports?

```bash
ss -tuln
```

### Q7. What is the difference between `ss` and `netstat`?

`ss` is the modern socket utility commonly preferred on Linux. `netstat` is an older utility from the `net-tools` package.

### Q8. How do you check whether port 8080 is listening?

```bash
sudo ss -tulnp | grep :8080
```

### Q9. How do you test whether a remote port is reachable?

```bash
nc -zv SERVER_IP 8080
```

### Q10. What is the purpose of `curl`?

`curl` transfers data using URLs and is commonly used to test websites, APIs, and HTTP/HTTPS connectivity.

### Q11. What is the difference between `curl -I` and `curl -v`?

`curl -I` sends a HEAD request and displays response headers. `curl -v` displays detailed connection and request information.

### Q12. What is the purpose of `wget`?

`wget` is used to download files from web servers.

### Q13. What is the purpose of `traceroute`?

It identifies intermediate network hops toward a destination and helps investigate routing paths.

### Q16. How do you check network interface statistics?
### Q14. What is the purpose of `tcpdump`?

`tcpdump` captures and displays network packets for traffic analysis and troubleshooting.

### Q15. How do you capture traffic on a particular port?

```bash
sudo tcpdump -i eth0 port 8080

```

```bash


ip -s link

```


### Q17. A server cannot reach the internet. Which commands would you use?


```bash
ip addr
ip route
ping -c 4 GATEWAY_IP
ping -c 4 8.8.8.8
nslookup google.com
```

I would check the interface, default route, gateway, external connectivity, and DNS configuration.

### Q18. An application is not accessible on port 8080. How would you troubleshoot it?

I would check whether the application is running, inspect listening ports with `ss`, test the application locally using `curl`, test remote port connectivity using `nc`, and inspect firewall and cloud security rules.

---


### Q19. How do you check active TCP connections?
## 18. Important Interview Points to Remember



```bash
- `ip addr` – Check IP addresses.
- `ip route` – Check routing information.
- `ip link` – Check network interfaces.
- `ping` – Test ICMP reachability.
- `nslookup`, `dig`, `host` – Troubleshoot DNS.
- `ss -tuln` – Check listening ports.
- `nc -zv` – Test TCP port connectivity.
- `curl` – Test HTTP and HTTPS endpoints.
- `wget` – Download files.
- `traceroute` – Inspect network hops.
- `tcpdump` – Capture network traffic.
- `ip -s link` – Inspect network interface statistics.

**DevOps interview tip:** Do not only memorize commands. Be able to explain what problem each command helps solve, how to interpret its output, and what you would investigate next.


ss -tn state established

```


### Q20. Why is `tcpdump` useful for DevOps engineers?


It helps inspect network packets and determine whether traffic reaches a server, whether responses are sent, and where communication may be failing.


