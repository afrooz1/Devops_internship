# Task 11 — Basic Network Configuration and Port Management

## 🎯 Overview

This task focuses on understanding basic Linux network configuration, monitoring network services and ports, and managing firewall rules using `iptables`.

The practical work was performed in **Ubuntu 24.04 LTS on WSL**.

---

## 🎯 Objectives

* Learn how to view network interface and IP configuration.
* Understand basic network routing and gateway information.
* Identify services listening on network ports.
* Use `nmap` to check the state of a specific port.
* Start and stop a temporary HTTP service on port `80`.
* Use `iptables` to create and manage firewall rules.
* Verify and safely remove firewall rules.

---

## 🛠️ Tools Used

* Ubuntu 24.04 LTS (WSL)
* `ip`
* `netstat`
* `nmap`
* Python 3
* `iptables`

---

# 1. View Network Configuration

The network interface configuration was inspected using:

```bash
ip addr
```

This command displays:

* Network interfaces
* IP addresses
* Subnet information
* Interface status

The main WSL network interface identified during the test was:

```text
eth0
```

For public documentation, the actual WSL IP address has been omitted.

The routing table was checked using:

```bash
ip route
```

The default gateway was also omitted from this public documentation.

### Purpose

These commands help identify how a Linux system is connected to a network and how network traffic is routed.

---

# 2. Check Listening Ports

To identify active network services, the following command was used:

```bash
sudo netstat -tulnp
```

The system showed several listening services.

Examples of services identified during the practical included:

|  Port | Service          |
| ----: | ---------------- |
|    22 | SSH              |
|    25 | SMTP             |
|  3306 | MySQL            |
|  5432 | PostgreSQL       |
| 33060 | MySQL X Protocol |

The exact local IP bindings have been omitted from this public README.

### Purpose

`netstat` helps identify:

* Listening network ports
* Active network connections
* TCP and UDP services
* Processes associated with network sockets when sufficient privileges are available

### Modern Alternative

On modern Linux systems, `ss` is commonly preferred over `netstat`:

```bash
ss -tulnp
```

---

# 3. Check Port 80 Before Starting a Service

Port `80` was selected because it is the standard port associated with HTTP.

The initial state was checked using:

```bash
sudo nmap -p 80 localhost
```

The result was:

```text
PORT   STATE  SERVICE

80/tcp closed http
```

### Result

Port 80 was **closed** because no service was listening on that port at the time of testing.

---

# 4. Start a Temporary HTTP Server

A temporary Python HTTP server was started on port 80:

```bash
sudo python3 -m http.server 80
```

The server displayed a message similar to:

```text
Serving HTTP on 0.0.0.0 port 80 ...
```

This indicates that the Python HTTP server was listening for HTTP connections on port 80.

> **Note:** Running a service on a privileged port such as port 80 generally requires elevated privileges on Linux.

---

# 5. Verify Port 80

While the Python HTTP server was running, Nmap was used again:

```bash
sudo nmap -p 80 localhost
```

The result showed:

```text
PORT   STATE SERVICE

80/tcp open  http
```

### Result

The port state changed from:

```text
CLOSED → OPEN
```

because a web service was now listening on port 80.

---

# 6. Stop the HTTP Server

The temporary Python HTTP server was stopped using:

```text
Ctrl + C
```

The server exited after receiving the keyboard interrupt.

Port 80 was then checked again:

```bash
sudo nmap -p 80 localhost
```

The result returned to:

```text
PORT   STATE  SERVICE

80/tcp closed http
```

### Port State Transition

```text
No HTTP service
       ↓
Port 80 CLOSED
       ↓
Start Python HTTP server
       ↓
Port 80 OPEN
       ↓
Stop Python HTTP server
       ↓
Port 80 CLOSED
```

This demonstrated how the presence or absence of a listening service affects the observed port state.

---

# 7. Install iptables

Initially, `iptables` was not available:

```text
sudo: iptables: command not found
```

It was installed using:

```bash
sudo apt install iptables -y
```

After installation, the firewall rules were inspected:

```bash
sudo iptables -L -n -v
```

The initial firewall configuration contained the standard chains:

```text
Chain INPUT (policy ACCEPT)

Chain FORWARD (policy ACCEPT)

Chain OUTPUT (policy ACCEPT)
```

---

# 8. Add a Firewall Rule for Port 80

A temporary firewall rule was added to drop incoming TCP traffic destined for port 80:

```bash
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```

The rule was verified using:

```bash
sudo iptables -L INPUT -n -v
```

The output included a rule similar to:

```text
DROP  tcp  --  *  *  0.0.0.0/0  0.0.0.0/0  tcp dpt:80
```

### Command Explanation

| Option       | Meaning                       |
| ------------ | ----------------------------- |
| `-A`         | Append a new rule             |
| `INPUT`      | Incoming network traffic      |
| `-p tcp`     | Apply the rule to TCP traffic |
| `--dport 80` | Match destination port 80     |
| `-j DROP`    | Drop matching packets         |

Therefore, the rule means:

> Drop incoming TCP traffic destined for port 80.

---

# 9. Remove the Firewall Rule

After inspecting the rule, it was removed using:

```bash
sudo iptables -D INPUT -p tcp --dport 80 -j DROP
```

The firewall configuration was checked again:

```bash
sudo iptables -L -n -v
```

The temporary port 80 rule was no longer present.

### Safety Note

Only the temporary port 80 firewall rule was intentionally added and removed during this practical.

Existing services were not intentionally modified.

> **Important:** Firewall commands can affect system connectivity. Always inspect a rule carefully before applying it, especially on remote systems where an incorrect rule could block SSH access.

---

# 10. Important Observation

The `iptables` rule showed:

```text
0 packets
0 bytes
```

This means that no traffic was recorded as matching the DROP rule during the test.

Therefore, this practical demonstrates that the firewall rule was successfully:

1. Created
2. Inspected
3. Removed

However, the results do **not** claim that the DROP rule itself was experimentally verified by blocking an Nmap connection.

The port 80 open/close behavior was independently demonstrated by starting and stopping the Python HTTP server.

This distinction is important because:

> A service listening on a port and a firewall rule controlling traffic to that port are two different concepts.

---

# 11. Key Commands

### Network Configuration

```bash
ip addr
ip route
```

### Check Listening Services

```bash
sudo netstat -tulnp
```

Modern alternative:

```bash
ss -tulnp
```

### Scan Port 80

```bash
sudo nmap -p 80 localhost
```

### Start HTTP Server

```bash
sudo python3 -m http.server 80
```

### Check Firewall Rules

```bash
sudo iptables -L -n -v
```

### Block Port 80

```bash
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```

### Remove Port 80 Rule

```bash
sudo iptables -D INPUT -p tcp --dport 80 -j DROP
```

---

# 12. What I Learned

Through this task, I learned:

* How to inspect Linux network interfaces and IP addresses.
* How to view the routing table and default gateway.
* How to identify services listening on network ports.
* How to use Nmap to check port states.
* How a running service can make a port available for connections.
* How to start and stop a temporary HTTP service using Python.
* How to create and inspect firewall rules using `iptables`.
* How to safely remove a temporary firewall rule.
* The difference between a **service listening on a port** and a **firewall rule controlling traffic to that port**.
* The importance of reviewing firewall rules before applying them.

---

# 13. Security Considerations

When publishing network troubleshooting work on GitHub:

* Avoid publishing real public IP addresses.
* Avoid publishing personal hostnames or dynamic DNS names.
* Avoid exposing unnecessary private/internal IP addresses.
* Do not publish credentials, passwords, API keys, or tokens.
* Avoid publishing sensitive network topology information.
* Only scan systems that you own or have explicit permission to test.
* Remove temporary firewall rules after completing a lab.
* Review listening services and disable unnecessary services where appropriate.

---

# 14. Conclusion

Task 11 provided practical experience with basic Linux network configuration, port management, service monitoring, and firewall administration.

I inspected network interfaces and routing information, identified listening services, and used Nmap to check the state of HTTP port 80. I then started a temporary Python HTTP server and observed the port transition from **closed to open**, followed by a return to the **closed** state after stopping the service.

I also installed and used `iptables` to create, inspect, and remove a temporary firewall rule for TCP port 80.

Overall, this task improved my understanding of **Linux networking, IP configuration, routing, ports, services, port scanning, and basic firewall management**.
