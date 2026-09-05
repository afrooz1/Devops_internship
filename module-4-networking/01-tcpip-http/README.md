# Task 1 — Understanding TCP/IP and HTTP/HTTPS

This task focuses on understanding the fundamentals of **TCP/IP networking**, the four layers of the TCP/IP model, and the differences between **HTTP and HTTPS**.

Practical testing was performed using the `curl` command-line tool to observe HTTP and HTTPS connections.

---

## 🎯 Objectives

* Understand the basics of TCP/IP networking.
* Learn the four layers of the TCP/IP model.
* Understand the purpose of each TCP/IP layer.
* Understand HTTP and HTTPS.
* Compare HTTP and HTTPS.
* Learn how to use `curl` for HTTP and HTTPS requests.
* Observe the differences between HTTP and HTTPS connections.

---

## 1. TCP/IP Basics

**TCP/IP (Transmission Control Protocol/Internet Protocol)** is a suite of networking protocols used to enable communication between devices over networks, including the Internet.

The TCP/IP model consists of four layers:

| Layer                       | Purpose                                                                | Example Protocols / Technologies |
| --------------------------- | ---------------------------------------------------------------------- | -------------------------------- |
| **Application Layer**       | Provides network services to applications and users.                   | HTTP, HTTPS, DNS, FTP, SMTP      |
| **Transport Layer**         | Provides communication between applications and manages data delivery. | TCP, UDP                         |
| **Internet Layer**          | Handles logical addressing and routing of packets.                     | IPv4, IPv6, ICMP                 |
| **Network Interface Layer** | Handles communication over local and physical networks.                | Ethernet, Wi-Fi, ARP             |

### Application Layer

The Application Layer provides network services directly to applications.

Examples:

* **HTTP / HTTPS** — Web communication
* **DNS** — Domain name resolution
* **FTP** — File transfer
* **SMTP** — Email communication

### Transport Layer

The Transport Layer provides end-to-end communication between applications.

* **TCP** — Reliable and connection-oriented.
* **UDP** — Connectionless and provides low-overhead communication without guaranteed delivery.

### Internet Layer

The Internet Layer handles logical addressing and packet routing between networks.

Examples:

* IPv4
* IPv6
* ICMP

### Network Interface Layer

The Network Interface Layer handles communication over local networks and physical networking technologies.

Examples:

* Ethernet
* Wi-Fi
* ARP

---

## 2. HTTP vs HTTPS

### HTTP

**HTTP (HyperText Transfer Protocol)** is an Application Layer protocol used for communication between clients and web servers.

* Default port: **80**
* Does not provide TLS encryption.
* Data is not protected by TLS while in transit.
* Commonly used for testing, demonstrations, and controlled environments.

### HTTPS

**HTTPS (HyperText Transfer Protocol Secure)** is HTTP transmitted over a secure **TLS (Transport Layer Security)** connection.

* Default port: **443**
* Uses TLS to protect communication.
* Provides confidentiality and integrity.
* Uses certificates for server authentication.
* Recommended for real-world websites and applications.

### HTTP vs HTTPS Comparison

| Feature               | HTTP                              | HTTPS                              |
| --------------------- | --------------------------------- | ---------------------------------- |
| Full Name             | HyperText Transfer Protocol       | HyperText Transfer Protocol Secure |
| Default Port          | 80                                | 443                                |
| TLS Encryption        | No                                | Yes                                |
| Confidentiality       | No TLS protection                 | Protected using TLS                |
| Server Authentication | No TLS certificate                | TLS certificate used               |
| Common Use            | Testing / controlled environments | Websites, APIs, logins, payments   |

---

## 3. Practical Testing with `curl`

`curl` is a command-line tool used to transfer data and communicate with servers using various network protocols, including HTTP and HTTPS.

### Check `curl` Version

```bash
curl --version
```

This command displays the installed version of `curl` along with supported protocols and features.

---

## 4. HTTP Request

The following command was used to send an HTTP request:

```bash
curl -v http://example.com
```

The `-v` option enables verbose output and displays detailed connection information.

### Observations

The HTTP request showed:

* Connection through **port 80**
* HTTP communication without TLS
* HTTP response headers
* HTML content returned by the server
* No TLS handshake

The server returned the **Example Domain** webpage.

---

## 5. HTTPS Request

The following command was used to send an HTTPS request:

```bash
curl -v https://example.com
```

### Observations

The HTTPS request showed:

* Connection through **port 443**
* TLS handshake
* Server certificate information
* Certificate verification
* Secure HTTP communication
* HTML content returned by the server

The webpage content was similar to the HTTP request, but the HTTPS connection was protected using TLS.

---

## 6. Practical Comparison

| Test  | Command                       | Port | Security      | Observation                                         |
| ----- | ----------------------------- | ---: | ------------- | --------------------------------------------------- |
| HTTP  | `curl -v http://example.com`  |   80 | No TLS        | HTTP connection and response observed               |
| HTTPS | `curl -v https://example.com` |  443 | TLS protected | TLS handshake and certificate verification observed |

---

## 🔄 7. HTTP vs HTTPS Communication

### HTTP

```text
Client
   |
   | HTTP Request
   | Port 80
   v
Web Server
   |
   | HTTP Response
   v
Client
```

HTTP does not establish a TLS connection, so HTTP traffic is not protected by TLS encryption.

### HTTPS

```text
Client
   |
   | TLS Handshake
   | Certificate Verification
   | Port 443
   v
Web Server
   |
   | Secure HTTP Communication
   v
Client
```

HTTPS establishes a secure TLS connection before HTTP application data is exchanged.

---

## 8. Key Learnings

Through this task, I learned:

* The basics of the TCP/IP model.
* The four TCP/IP layers and their responsibilities.
* The difference between TCP and UDP.
* The role of IP addressing and routing.
* How HTTP communication works.
* How HTTPS protects HTTP using TLS.
* The difference between port **80** and port **443**.
* How to use `curl` for network testing.
* How to inspect HTTP and HTTPS connection details using verbose `curl` output.
* How TLS certificates are involved in HTTPS server authentication.

---

## 🛠️ Tools Used

* **Linux / Ubuntu**
* **Terminal**
* **curl**
* **HTTP**
* **HTTPS**
* **TCP/IP**

---

## 📌 Conclusion

This task provided practical knowledge of **TCP/IP networking and web communication**.

The TCP/IP model uses four layers — **Application, Transport, Internet, and Network Interface** — to organize how applications and devices communicate across networks.

The `curl` tests demonstrated that **HTTP normally uses port 80 without TLS**, while **HTTPS normally uses port 443 and establishes a secure TLS connection**.

For real-world websites and applications, **HTTPS should be preferred**, especially when transmitting sensitive information.
