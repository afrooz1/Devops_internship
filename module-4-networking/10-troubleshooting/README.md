# Task 10 — Network Troubleshooting Tools

## 🎯 Objective

The objective of this task was to learn and practice basic network troubleshooting tools:

* `ping` — Check network connectivity.
* `traceroute` — Trace the path packets take to a destination.
* `netstat` — View listening ports and network connections.
* `nmap` — Scan a host for open ports and services.

---

## 1. Ping

### Command

```bash
ping -c 4 google.com
```

### Result

```text
PING google.com (...) 56(84) bytes of data.

64 bytes from ...: icmp_seq=1 ttl=116 time=33.7 ms
64 bytes from ...: icmp_seq=2 ttl=116 time=34.3 ms
64 bytes from ...: icmp_seq=3 ttl=116 time=34.4 ms
64 bytes from ...: icmp_seq=4 ttl=116 time=38.9 ms

4 packets transmitted, 4 received, 0% packet loss

rtt min/avg/max/mdev = 33.689/35.312/38.881/2.077 ms
```

### Interpretation

* `google.com` was successfully resolved to an IP address.
* All 4 packets were successfully received.
* Packet loss was **0%**.
* The average response time was approximately **35.3 ms**.
* This confirms that the destination was reachable from the system at the time of testing.

> **Security note:** The original destination IP address has been removed from this public README.

---

## 2. Traceroute

### Command

```bash
traceroute google.com
```

### Result

The traceroute successfully reached the destination in **9 hops**.

For security and privacy, the actual intermediate IP addresses and network-specific hostnames have been omitted.

```text
Hop 1   Local network
Hop 2   Private/network gateway
Hop 3   ISP network
Hop 4   Private network
Hop 5   Private network
Hop 6   Private network
Hop 7   Private network
Hop 8   Google network
Hop 9   Destination
```

### Interpretation

* The first hop represented the local network/gateway.
* Several intermediate network hops were encountered.
* The destination was reached at the **9th hop**.
* The final response time was approximately **36 ms**.
* Traceroute helps visualize the path packets take between the local system and a remote destination.

> **Note:** Some routers may not respond to traceroute probes. This can result in `* * *` for individual hops and does not necessarily indicate a network failure.

---

## 3. Netstat

### Command

```bash
netstat -tulnp
```

### Result

The command showed several listening TCP and UDP ports on the local system.

For a public portfolio, the exact local addresses and complete service inventory have been omitted.

Example categories observed during the test included:

```text
TCP   ...:22       LISTEN
TCP   ...:25       LISTEN
TCP   ...:3306     LISTEN
TCP   ...:5432     LISTEN
TCP   ...:53       LISTEN
UDP   ...:53
```

### Important Ports

|  Port | Protocol | Purpose          |
| ----: | -------- | ---------------- |
|    22 | TCP      | SSH              |
|    25 | TCP      | SMTP             |
|    53 | TCP/UDP  | DNS              |
|  3306 | TCP      | MySQL            |
|  5432 | TCP      | PostgreSQL       |
| 33060 | TCP      | MySQL X Protocol |

### Interpretation

The output showed that several network services were listening for connections on the local system.

Important services identified included:

* **22** — SSH
* **25** — SMTP
* **53** — DNS
* **3306** — MySQL
* **5432** — PostgreSQL
* **33060** — MySQL X Protocol

The command also displayed a warning because it was not executed with root privileges:

```text
(No info could be read for "-p": geteuid()=1000 but you should be root.)
```

This means `netstat` could display network information, but it could not show complete process/Program ownership information for some connections.

For more detailed information, the command can be run with elevated privileges:

```bash
sudo netstat -tulnp
```

### Modern Alternative

`ss` is commonly preferred on modern Linux systems:

```bash
ss -tulnp
```

---

## 4. Nmap

### Command

```bash
nmap localhost
```

### Result

```text
Starting Nmap 7.94SVN

Nmap scan report for localhost (127.0.0.1)

Host is up.

Not shown: 996 closed tcp ports (conn-refused)

PORT      STATE  SERVICE

22/tcp    open   ssh
25/tcp    open   smtp
3306/tcp  open   mysql
5432/tcp  open   postgresql
```

### Interpretation

Nmap detected that the local machine was **up** and identified four open TCP ports:

|     Port | State | Service    |
| -------: | ----- | ---------- |
|   22/tcp | Open  | SSH        |
|   25/tcp | Open  | SMTP       |
| 3306/tcp | Open  | MySQL      |
| 5432/tcp | Open  | PostgreSQL |

Nmap also reported that **996 TCP ports were closed** in its default scan range.

This demonstrates how Nmap can be used to identify accessible network services on a host.

### Useful Nmap Commands

Scan a specific port:

```bash
nmap -p 22 localhost
```

Scan multiple ports:

```bash
nmap -p 22,80,443 localhost
```

Detect service versions:

```bash
nmap -sV localhost
```

Scan a specific host:

```bash
nmap <target-ip>
```

> Only scan systems that you own or have explicit permission to test.

---

## 5. Tools Summary

| Tool         | Purpose                              | Result                                    |
| ------------ | ------------------------------------ | ----------------------------------------- |
| `ping`       | Test network connectivity            | Destination reachable with 0% packet loss |
| `traceroute` | Trace network path                   | Destination reached in 9 hops             |
| `netstat`    | View listening ports and connections | Multiple network services identified      |
| `nmap`       | Scan ports and services              | 4 open TCP ports detected                 |

---

## 6. Key Learnings

* `ping` can be used to test basic network reachability and measure response time.
* `traceroute` helps identify the path packets take through a network.
* `netstat` can display listening ports and active network connections.
* `ss` is a modern alternative to `netstat` on Linux.
* `nmap` can identify open ports and services on a host.
* Different network services use different TCP or UDP ports.
* Open ports should be reviewed and secured when they are not required.
* Network troubleshooting tools provide valuable information when diagnosing connectivity and service-related problems.
* Network information from personal systems should be sanitized before being published publicly.

---

## 7. Security Considerations

When documenting network troubleshooting activities for a public repository:

* Avoid publishing real public IP addresses.
* Avoid publishing personal hostnames or dynamic DNS names.
* Avoid publishing internal/private network addresses unnecessarily.
* Do not publish VPN addresses, internal infrastructure details, or network topology.
* Do not expose usernames, credentials, API keys, or authentication tokens.
* Only scan systems that you own or have permission to test.
* Review listening services and close unnecessary ports.
* Use a firewall to restrict unnecessary network access.

---

## 8. Conclusion

In this task, I practiced four important network troubleshooting tools: **Ping, Traceroute, Netstat, and Nmap**.

The `ping` test confirmed successful connectivity with **0% packet loss** and an average response time of approximately **35 ms**. `traceroute` demonstrated how packets travel through multiple network hops before reaching a destination.

Using `netstat`, I identified several listening network services, including SSH, SMTP, DNS, MySQL, and PostgreSQL. Finally, `nmap localhost` was used to scan the local system and identify **four open TCP ports**.

These tools provide practical methods for diagnosing connectivity problems, analyzing network paths, checking listening services, and identifying exposed ports. They are fundamental tools for both **network administration and cybersecurity troubleshooting**.
