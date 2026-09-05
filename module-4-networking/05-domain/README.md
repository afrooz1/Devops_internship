# Task 5 — Domains and Subdomains

This task focuses on understanding the structure of **domain names and subdomains**, configuring a test hostname, and practicing hostname-to-IP resolution using DNS and `ping`.

---

## 🎯 Objective

The objectives of this task were to:

* Understand domain names and subdomains.
* Learn how subdomains are structured.
* Understand the relationship between domains and subdomains.
* Configure a test hostname.
* Practice hostname-to-IP resolution.
* Use `ping` to test hostname resolution and network connectivity.

---

# 1. Domain Name

A **domain name** is a human-readable name used to identify a website or network resource.

Example:

```text
example.com
```

A domain can be used as the main address for a website, application, or other network service.

---

# 2. Subdomain

A **subdomain** is a hostname created under an existing domain.

Example:

```text
api.example.com
```

In this example:

```text
example.com      → Domain
api              → Subdomain
api.example.com  → Complete hostname
```

Subdomains are commonly used to organize different services, applications, and environments.

### Common Examples

```text
www.example.com
api.example.com
mail.example.com
test.example.com
dev.example.com
```

For example:

* `www` may be used for a website.
* `api` may be used for an API service.
* `mail` may be used for mail services.
* `test` or `dev` may be used for testing and development environments.

---

# 3. Domain vs Subdomain

| Feature      | Domain                  | Subdomain                         |
| ------------ | ----------------------- | --------------------------------- |
| Example      | `example.com`           | `api.example.com`                 |
| Purpose      | Main domain identity    | Organizes services under a domain |
| Registration | Domain is registered    | Created/configured through DNS    |
| Structure    | Main domain             | Prefix + domain                   |
| Example Use  | Website or organization | API, mail, testing, development   |

---

# 4. Test Domain and Subdomain

For this exercise, a test hostname was used to understand how subdomains can be configured and resolved.

To keep personal infrastructure information out of the public repository, the actual hostname has been replaced with a documentation-safe example.

### Domain

```text
example.com
```

### Example Subdomain

```text
api.example.com
```

The subdomain can be configured through DNS to point to an appropriate server or service.

> **Security Note:** Personal hostnames and public IP addresses used during private testing should generally be omitted from public learning documentation unless there is a specific reason to publish them.

---

# 5. Hostname and IP Resolution

DNS translates human-readable hostnames into IP addresses.

A hostname can be tested using:

```bash
ping example.com
```

The command attempts to resolve the hostname and then sends ICMP packets to the resolved address.

Example output may contain information similar to:

```text
PING example.com (...) ...
64 bytes from ...: icmp_seq=1 ...
64 bytes from ...: icmp_seq=2 ...
```

The exact IP address returned by a public domain can change over time, so it should not be treated as a permanent value.

---

# 6. Testing a Subdomain

A configured subdomain can be tested using:

```bash
ping api.example.com
```

If the DNS record is configured correctly and the destination responds to ICMP, the command can display:

* Resolved IP address
* Response time
* Packet loss
* Connectivity status

Example:

```text
PING api.example.com (...) ...
64 bytes from ...: icmp_seq=1 ...
64 bytes from ...: icmp_seq=2 ...
```

> **Note:** A successful DNS resolution does not necessarily mean that the server will respond to `ping`. ICMP may be blocked by the destination or network firewall.

---

# 7. DNS Resolution

The basic relationship between a hostname and an IP address can be represented as:

```text
User enters hostname
        |
        v
   DNS Resolution
        |
        v
     IP Address
        |
        v
Network Connection
```

For example:

```text
api.example.com
       |
       | DNS
       v
  203.0.113.10
```

The IP address above belongs to the documentation range reserved for examples and is not a real production server address.

---

# 8. Domain vs Subdomain Resolution

| Hostname          | Example IP     | Purpose             |
| ----------------- | -------------- | ------------------- |
| `example.com`     | `203.0.113.10` | Main domain example |
| `api.example.com` | `203.0.113.20` | Subdomain example   |

These example addresses are used only for documentation.

In a real environment, DNS may resolve different hostnames to the same IP address, different IP addresses, or other types of DNS records depending on the configuration.

---

# 9. Commands Used

### Test a Domain

```bash
ping example.com
```

### Test a Subdomain

```bash
ping api.example.com
```

### Check DNS Resolution Directly

The `dig` command can also be used to inspect DNS resolution:

```bash
dig example.com
```

For a subdomain:

```bash
dig api.example.com
```

Another commonly available command is:

```bash
nslookup example.com
```

These tools are useful when troubleshooting DNS independently from ICMP connectivity.

---

# 🔐 10. Security and Privacy Considerations

When publishing networking practicals to a public GitHub repository, avoid exposing unnecessary personal infrastructure information.

The following should generally be sanitized unless intentionally published:

* Personal hostnames.
* Public IP addresses.
* Internal IP addresses and network details.
* Private DNS names.
* DNS verification records.
* Credentials and authentication information.
* Internal company infrastructure.

For this README, personal testing information has been replaced with documentation-safe examples.

---

# 📚 11. Key Learnings

Through this task, I learned:

* A **domain** identifies a main internet namespace or resource.
* A **subdomain** is a hostname created under a domain.
* Subdomains can be used for APIs, websites, mail, testing, and development environments.
* DNS resolves hostnames to IP addresses.
* `ping` can be used to observe hostname resolution and test ICMP connectivity.
* `dig` and `nslookup` can be used to inspect DNS resolution directly.
* DNS resolution and network connectivity are related but are not the same thing.
* A hostname can resolve successfully even when ICMP traffic is blocked.
* Dynamic DNS services can be useful for testing and managing hostnames whose IP addresses may change.

---

# 🛠️ Tools and Technologies

* **Linux / Ubuntu**
* **Terminal**
* **DNS**
* **Domain Names**
* **Subdomains**
* **ping**
* **dig**
* **nslookup**
* **Dynamic DNS**

---

# 📌 Conclusion

This task provided practical knowledge of **domains, subdomains, DNS resolution, and hostname-to-IP mapping**.

A domain represents the main namespace, while subdomains allow different services and environments to be organized under that domain.

Using tools such as `ping`, `dig`, and `nslookup`, hostname resolution and network connectivity can be investigated.

The exercise also demonstrated an important networking concept: **successful DNS resolution does not always mean that the destination will respond to ICMP traffic**.

Understanding domains, subdomains, and DNS is important for **network administration, DevOps, cloud infrastructure, and cybersecurity**.
