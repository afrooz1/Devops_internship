# Task 7 — SSL/TLS Basics

## Objective

The objective of this task was to understand **SSL/TLS**, learn their role in network security, examine the differences between SSL and TLS, and use `openssl` to inspect the TLS certificate and security information of an HTTPS website.

---

# 1. What are SSL and TLS?

**SSL (Secure Sockets Layer)** and **TLS (Transport Layer Security)** are cryptographic protocols used to secure communication over a network.

Modern systems use **TLS**, while SSL is deprecated and should no longer be used.

TLS provides:

* **Encryption** — Protects data from being read by unauthorized users.
* **Authentication** — Helps verify the identity of the server through digital certificates.
* **Data Integrity** — Helps detect unauthorized modification of data during transmission.

TLS is commonly used with **HTTPS** to secure communication between a client and a web server.

```text
Client / Browser
       |
       | Encrypted HTTPS Connection
       |       using TLS
       |
       ↓
   Web Server
```

---

# 2. Purpose of SSL/TLS

The main purpose of TLS is to establish a secure communication channel between a client and a server.

Without encryption, sensitive information such as:

* Passwords
* Login credentials
* Personal information
* Payment information

could potentially be intercepted or modified while being transmitted.

With TLS, communication is encrypted and protected against unauthorized reading or modification.

---

# 3. SSL vs TLS

| Feature             | SSL                  | TLS                      |
| ------------------- | -------------------- | ------------------------ |
| Full Name           | Secure Sockets Layer | Transport Layer Security |
| Status              | Deprecated           | Modern security protocol |
| Security            | Older and vulnerable | Stronger and more secure |
| Latest Version      | SSL 3.0              | TLS 1.3                  |
| Modern HTTPS        | Not recommended      | Used                     |
| Modern Cryptography | Limited              | Supported                |
| Forward Secrecy     | Limited              | Supported by modern TLS  |

TLS was developed as the successor to SSL and introduced improved security mechanisms and cryptographic algorithms.

**Modern systems should use TLS 1.2 or TLS 1.3, with TLS 1.3 preferred where supported.**

---

# 4. Why SSL is Considered Insecure

SSL is no longer considered secure because its older versions contain known security weaknesses.

### POODLE

The **POODLE** vulnerability affected SSL 3.0 and demonstrated that weaknesses in older protocol versions could potentially be exploited to recover protected information under certain conditions.

### Weak Cryptographic Algorithms

Older SSL implementations supported outdated and weak cryptographic algorithms that are no longer considered secure.

### Lack of Modern Security Features

Modern TLS versions provide stronger cryptographic mechanisms and security improvements that are not available in older SSL versions.

### Deprecated Protocol

SSL 2.0 and SSL 3.0 are deprecated and should not be used for modern secure communication.

**Modern applications should use TLS 1.2 or TLS 1.3.**

---

# 5. OpenSSL Installation and Version

OpenSSL was available in the Ubuntu WSL environment.

Command used:

```bash
openssl version
```

### Result

```text
OpenSSL 3.0.13 30 Jan 2024
```

This confirms that OpenSSL was installed and available for testing.

> **Note:** The version shown above represents the version installed in the testing environment at the time the practical was performed.

---

# 6. Connecting to an HTTPS Website Using OpenSSL

The following command was used to establish a TLS connection with `example.com`:

```bash
openssl s_client -connect example.com:443 -servername example.com
```

### Command Explanation

| Option                     | Purpose                              |
| -------------------------- | ------------------------------------ |
| `s_client`                 | Creates an SSL/TLS client connection |
| `-connect example.com:443` | Connects to the HTTPS port           |
| `-servername example.com`  | Sends the hostname using SNI         |

### What is SNI?

**SNI (Server Name Indication)** allows the client to specify the hostname it wants to connect to during the TLS handshake.

This is important because a single server can host multiple HTTPS websites, each potentially using a different certificate.

---

# 7. TLS Certificate Information

The OpenSSL output returned information about the server certificate.

### Certificate Subject

```text
CN = example.com
```

The certificate's subject identifies the domain for which the certificate was issued.

### Certificate Issuer

```text
CN = Cloudflare TLS Issuing ECC CA 3
```

The certificate was issued by an intermediate Certificate Authority (CA).

### Certificate Validity

The test output showed a validity period similar to:

```text
NotBefore: Jul 29 22:10:08 2026 GMT
NotAfter:  Oct 27 22:17:21 2026 GMT
```

This indicates the period during which the certificate is valid.

> **Note:** Certificate details such as issuer and validity dates can change when a website renews or replaces its certificate. The values above represent the certificate observed during this practical exercise.

---

# 8. Certificate Chain

The OpenSSL output showed a certificate chain containing the server certificate and its issuing Certificate Authorities.

Conceptually, certificate validation follows a chain such as:

```text
Server / Leaf Certificate
          ↓
Intermediate CA Certificate
          ↓
Trusted Root CA
```

The certificate chain helps the client establish whether the server certificate can be trusted.

In the observed test output, the chain included Cloudflare and SSL.com certificate authorities.

---

# 9. TLS Connection Details

The OpenSSL connection negotiated the following parameters during the test.

### TLS Version

```text
TLSv1.3
```

TLS 1.3 is a modern TLS protocol version that provides improved security and a more efficient handshake compared with older TLS versions.

### Cipher Suite

```text
TLS_AES_256_GCM_SHA384
```

This cipher suite uses:

* **AES-256-GCM** for authenticated encryption.
* **SHA-384** for hashing within the TLS cryptographic construction.

### Server Public Key

```text
256 bit
```

The certificate used a 256-bit elliptic-curve public key.

### Server Temporary Key

```text
X25519, 253 bits
```

X25519 was used for ephemeral key exchange, providing properties that support forward secrecy.

### Signature

```text
ECDSA
```

ECDSA was used for the certificate/signature-related cryptographic operations observed in the connection.

### Verification

```text
Verify return code: 0 (ok)
```

This indicates that OpenSSL successfully verified the certificate chain during the test.

---

# 10. Practical Results

| Item                     | Result                          |
| ------------------------ | ------------------------------- |
| OpenSSL Version          | 3.0.13                          |
| Website Tested           | `example.com`                   |
| HTTPS Port               | 443                             |
| TLS Version              | TLS 1.3                         |
| Cipher Suite             | `TLS_AES_256_GCM_SHA384`        |
| Certificate Subject      | `example.com`                   |
| Certificate Issuer       | Cloudflare TLS Issuing ECC CA 3 |
| Certificate Verification | Successful                      |
| Verify Return Code       | `0 (ok)`                        |
| Server Public Key        | 256-bit                         |
| Temporary Key            | X25519                          |

> **Note:** The certificate issuer and certificate dates are a snapshot of the website's certificate at the time of testing and may change in future.

---

# 11. Commands Used

## Check OpenSSL Version

```bash
openssl version
```

## Establish TLS Connection

```bash
openssl s_client -connect example.com:443 -servername example.com
```

## Display Certificate Subject and Issuer

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer
```

## Display Certificate Validity

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null 2>/dev/null | openssl x509 -noout -dates
```

---

# 12. Key Learnings

* SSL and TLS are cryptographic protocols used to secure network communication.
* TLS is the modern successor to SSL.
* SSL is deprecated because of known vulnerabilities and outdated security mechanisms.
* HTTPS uses TLS to protect web communication.
* TLS provides encryption, authentication, and data integrity.
* `openssl s_client` can be used to inspect an HTTPS server's TLS connection.
* Digital certificates contain information about the server and the Certificate Authority that issued them.
* Certificate chains help establish trust between a server certificate and a trusted root CA.
* TLS 1.3 provides modern security and performance improvements.
* The tested connection successfully negotiated `TLS_AES_256_GCM_SHA384`.
* OpenSSL reported `Verify return code: 0 (ok)`, indicating successful certificate verification during the test.

---

# Conclusion

In this task, I learned the fundamentals of **SSL/TLS and their importance in network security**.

Using OpenSSL, I established a TLS connection to `example.com` and inspected its certificate information, certificate chain, and negotiated security parameters.

The connection successfully negotiated **TLS 1.3** with the cipher suite **`TLS_AES_256_GCM_SHA384`**, and certificate verification returned **`0 (ok)`**.

This practical exercise demonstrated how HTTPS uses TLS to provide **encryption, authentication, and data integrity** for secure network communication.
