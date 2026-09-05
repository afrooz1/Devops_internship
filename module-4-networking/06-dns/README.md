# Task 6 — DNS and DNS Records

## Objective

The objective of this task was to understand **DNS (Domain Name System)**, common DNS record types, and how domain names are translated into IP addresses.

The practical work was performed using `dig` and `nslookup` to query different DNS records.

---

## 1. What is DNS?

**DNS (Domain Name System)** is a distributed naming system that translates human-readable domain names into IP addresses and provides other information about network services.

For example:

```text
example.com
     ↓
    DNS
     ↓
203.0.113.10
```

This allows users and applications to use domain names instead of remembering IP addresses.

### Role of a DNS Server

A DNS server:

* Receives DNS queries from clients.
* Finds the requested DNS record.
* Returns the requested DNS information.
* Helps applications locate websites, mail servers, and other network services.

---

## 2. Common DNS Record Types

| Record    | Purpose                                                                     |
| --------- | --------------------------------------------------------------------------- |
| **A**     | Maps a hostname to an IPv4 address.                                         |
| **AAAA**  | Maps a hostname to an IPv6 address.                                         |
| **CNAME** | Creates an alias that points to another hostname.                           |
| **MX**    | Specifies mail servers responsible for email delivery.                      |
| **TXT**   | Stores text information, commonly used for verification and email security. |

### A Record

An **A record** maps a domain or hostname to an IPv4 address.

Example:

```text
example.com → 203.0.113.10
```

### AAAA Record

An **AAAA record** maps a hostname to an IPv6 address.

Example:

```text
example.com → 2001:db8::10
```

### CNAME Record

A **CNAME** record creates an alias that points to another hostname.

Example:

```text
www.example.com → example.com
```

### MX Record

An **MX (Mail Exchange)** record specifies the mail servers responsible for receiving email for a domain.

Example:

```text
example.com → mail.example.com
```

MX records also contain a **priority value**. Lower numbers have higher priority.

### TXT Record

A **TXT** record stores text information associated with a domain.

Common uses include:

* Domain verification
* SPF email security
* DKIM-related information
* Other domain ownership or service verification

---

# 3. DNS Queries Using `dig`

`dig` is a command-line tool used to query DNS servers and inspect DNS records.

During the practical exercise, different DNS record types were queried.

> **Security note:** Real personal domains, public IP addresses, and verification tokens have been replaced with documentation-safe examples in this public README.

---

## 3.1 A Record

Command:

```bash
dig A example.com
```

Example result:

```text
example.com.    IN    A    203.0.113.10
```

The A record maps the hostname to an IPv4 address.

**Status:** Successful

---

## 3.2 AAAA Record

Command:

```bash
dig AAAA example.com
```

Example result:

```text
ANSWER: 0
```

No AAAA record was returned in this query.

This demonstrates that a DNS query can be successful even when the requested record type does not exist.

**Status:** Query successful — no IPv6 record found

---

## 3.3 MX Record

Command:

```bash
dig example.com MX
```

Example result:

```text
example.com.    IN    MX    10    mail.example.com.
```

The MX record identifies the mail server responsible for receiving email for the domain.

**Status:** Successful

---

## 3.4 TXT Record

Command:

```bash
dig example.com TXT
```

Example result:

```text
"example-verification-token"
```

TXT records can contain domain verification or email-security information.

For security and privacy reasons, real verification tokens from personal domains should not be published unnecessarily in a public repository.

**Status:** Successful

---

## 3.5 CNAME Record

Command:

```bash
dig www.example.com CNAME
```

Example result:

```text
ANSWER: 0
```

If no CNAME record is returned, the queried hostname does not have a CNAME record.

**Status:** Query successful — no CNAME record found

---

# 4. DNS Queries Using `nslookup`

`nslookup` is another command-line utility that can be used to perform DNS lookups.

## A Record

Command:

```bash
nslookup example.com
```

Example result:

```text
Name:    example.com
Address: 203.0.113.10
```

This confirms the IPv4 address returned by DNS.

---

## MX Record

Command:

```bash
nslookup -type=MX example.com
```

Example result:

```text
example.com    mail exchanger = 10 mail.example.com.
```

This displays the mail server configured for the domain.

---

## A Record with Type Specified

Command:

```bash
nslookup -type=A example.com
```

Example result:

```text
Name:    example.com
Address: 203.0.113.10
```

---

# 5. DNS Resolution of a Hostname

A hostname can be resolved using commands such as:

```bash
ping example.com
```

or:

```bash
dig example.com A
```

The DNS resolver translates the hostname into an IP address.

For documentation purposes, the following example uses the reserved documentation address:

```text
example.com → 203.0.113.10
```

The `203.0.113.0/24` range is reserved for documentation and examples, making it suitable for public technical documentation.

---

# 6. Practical Results

| DNS Record | Command                     | Result                           |
| ---------- | --------------------------- | -------------------------------- |
| **A**      | `dig A example.com`         | IPv4 address returned            |
| **AAAA**   | `dig AAAA example.com`      | No record returned               |
| **MX**     | `dig example.com MX`        | Mail server information returned |
| **TXT**    | `dig example.com TXT`       | TXT record(s) returned           |
| **CNAME**  | `dig www.example.com CNAME` | No CNAME record returned         |

---

# 7. Commands Used

### A Record

```bash
dig A example.com
```

### AAAA Record

```bash
dig AAAA example.com
```

### MX Record

```bash
dig example.com MX
```

### TXT Record

```bash
dig example.com TXT
```

### CNAME Record

```bash
dig www.example.com CNAME
```

### Short A Record Lookup

```bash
dig example.com A +short
```

### Using `nslookup`

```bash
nslookup example.com
```

```bash
nslookup -type=A example.com
```

```bash
nslookup -type=MX example.com
```

---

# 8. Security and Privacy Considerations

When documenting DNS practical work in a public GitHub repository:

* Avoid publishing personal hostnames unless there is a specific reason.
* Avoid exposing real public IP addresses unnecessarily.
* Do not publish domain verification tokens.
* Do not publish API keys, passwords, or other credentials.
* Avoid exposing internal infrastructure details.
* Use `example.com` and documentation IP ranges such as `203.0.113.0/24` for examples.
* Remember that DNS records themselves are generally public, but unnecessary infrastructure details can still provide useful information to attackers.

---

# 9. Key Learnings

* DNS translates domain names into IP addresses and provides other DNS information.
* **A** records are used for IPv4 addresses.
* **AAAA** records are used for IPv6 addresses.
* **CNAME** records create aliases for hostnames.
* **MX** records specify mail servers.
* **TXT** records store text information such as verification and email-security data.
* `dig` can be used to query specific DNS record types.
* `nslookup` can also be used to perform DNS lookups.
* A successful DNS query does not always mean that the requested record exists.
* DNS is an important part of web access, email delivery, and network troubleshooting.

---

# Conclusion

In this task, I learned how **DNS works and how different DNS record types provide different types of information**.

Using `dig` and `nslookup`, I practiced querying **A, AAAA, MX, TXT, and CNAME records**.

The practical exercise demonstrated how A records provide IPv4 addresses, AAAA records provide IPv6 addresses, MX records identify mail servers, TXT records store domain-related text information, and CNAME records provide hostname aliases.

This practical work improved my understanding of **DNS resolution, DNS record types, domain configuration, and command-line DNS troubleshooting**.
